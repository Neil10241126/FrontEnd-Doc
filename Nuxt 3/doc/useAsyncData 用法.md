# useAsyncData 用法
---

**專寫日期 : 2024/11/07**

**更新日期 : 2024/11/15**

**相關連結 : [useAsyncData](https://nuxt.com/docs/api/composables/use-async-data)**

---

使用 `useAsyncData` 這個包裝方法而不是直接使用 `$fetch` 來獲取資料，可以避免 Client 端也重覆請求，導致`兩次請求發生`

## 用法 Usage :

```html
<script setup lang="ts">
const { data, status, error, refresh, clear } = await useAsyncData('usersFetch', /* key值 */
  () => $fetch('api/users', { /* $fetch 的可選參數... */ }),
  {
    /* useAsyncData 提供的可選參數 */
  }
)
</script>
```

### $fetch 可選參數 & 攔截器

```js
$fetch('/api/users', {
  /* 基礎參數 */
  method: "GET",           // HTTP 請求的方法，GET(default)、POST 、 DELETE、PUT。
  query: { id: 123 },      // 將參數透過？的方式帶到 URL 上
  params: {},              // 將參數帶到 URL 上
  body: { some: "json" },  // Request body
  headers: {},             // Request headers
  baseURL: "/",            // 基本的 API URL 路徑

  /* 攔截器參數 */
  onRequest({ request, options }) {},
  onRequestError({ request, options, error }) {},
  onResponse({ request, response, options }) {},
  onResponseError({ request, response, options }) {}
})
```

📌 $fetch 是基於 ofetch 的封裝，可參考文檔 : [ofetch](https://github.com/unjs/ofetch)


### 搭配 refreshNuxtData 重新獲取資料

如果 Client 端需要重新獲取資料時，除了使用 `refresh` 外，還可以使用 `refreshNuxtData` 這個 `utils` 來處理重新獲取這個行為

```html
<script setup lang="ts">
const { status, data: count } = await useAsyncData('count', () => $fetch('/api/count'))

// 將 key 值存入 refreshNuxtData() 當中，觸發 refresh
const refresh = () => refreshNuxtData('count')
</script>

<template>
  <div>
    {{ status === 'pending' ? 'Loading' : count }}
  </div>
  <button @click="refresh">Refresh</button>
</template>
```

📌 更多關於 refreshNuxtData 說明 : [refreshNuxtData](https://nuxt.com/docs/api/utils/refresh-nuxt-data#refresh-specific-data)

## 回傳值 Return Value :

- `data` : 非同步函式在發出請求後回傳的資料。
- `status` : 資料請求的狀態 ( `idle 尚未開始`、`peding 等待回應`、`success 請求成功`、`error 請求失敗` )。
- `error` : : 資料請求取得失敗，會回傳相關錯誤物件，成功時會回傳 null。
- `refresh` : 重新取得資料方法，用於更新資料。
- `clear` : 清除獲取 `data` 資料的方法，此時 `data` 將為 `null`，`error` 將為 `null`，`status` 將為 `idle`。

> [!NOTE]
> 當在 Vue 中的 `<script setup>` 環境調用 `data`、`status`、`error` 時應使用 `.value` 來存取。 而 `refresh`、`clear` 則為函式方法。

## 參數 Params :

useAsyncData 是一個 Nuxt 所包裝的獲取非同步資料的方法，包含三個參數 :

- `Key` : key 是`唯一值`，`防止 Server 與 Client 端兩次觸發獲取資料行為`
- `() => $fetch(url, {})` : 發出請求的非同步函式 `handler`，必須將 $fetch 請求的 response 回傳給 useAsyncData。也可以在第二參數放入參數與攔截器。
- `Options` : 屬於 useAsyncData 提供的參數，設定伺服器端處理資料的細部功能 :
  - `server` : 為布林值，表示是否在伺服器端處理資料的請求，預設為 `true`。當設為 `false` 時，請求只會在客戶端執行。
  - `transform` : 是一個函式，用於修改 `handler` 的`請求結果`並回傳
  - `pick` : 為陣列格式，填入`屬性名稱`，從 `handler` 回傳的資料中取出指定屬性名稱資料。
  
### transform 參數

程式碼範例 : 處理回傳的 response 並將 results 資料取出。

```html
<script setup>
const { data } = await useAsyncData('userResultes', () => {
    return $fetch('https://randomuser.me/api/');
  },
  {
    transform: (response) => {
      // 修改 response 的結果，取出 results 資料並回傳物件結構
      const [resultObject] = response.results

      // 一定要 return 資料且不能為 undefined，否則在 data 會取不到資料並出現警告 
      return response.results[0];
    }
  }
);
</script>
```

取回的 results 資料:

```json
{
  gender: 'male',
  name: { title: 'Mr', first: 'Javier', last: 'Ureña' },
  email: 'javier.urena@example.com',
,
  dob: { date: '1989-09-17T05:46:14.931Z', age: 35 },
  registered: { date: '2015-02-15T16:10:40.807Z', age: 9 },
  phone: '(613) 184 5146',
  cell: '(616) 529 1921',
  id: { name: 'NSS', value: '40 37 26 2729 9' },
  nat: 'MX',
  // ...以下省略
}
```


### pick 參數

程式碼範例 : 嘗試只取得 data 資料中的 info 屬性資料。

```html
<script setup>
// 用 pick 參數從 handler 函式回傳的資料中，從物件取出 info 屬性的資料
const { data } = await useAsyncData('userInfo', () => {
    return $fetch('https://randomuser.me/api/', {
      query: { seed: 'be579ffd62ede3ce' },
    });
  },
  {
    pick: ['info']
  }
);
</script>
```

此時 data 獲取的資料結果會是 :

```json
{
  "info": {
    "seed": "be579ffd62ede3ce",
    "results": 1,
    "page": 1,
    "version": "1.4"
  }
}
```