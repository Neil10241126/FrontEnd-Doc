# useFetch 用法
---

**專寫日期 : 2024/11/07**

**更新日期 : 2024/11/15**

**相關連結 : [useFetch](https://nuxt.com/docs/api/composables/use-fetch)**

 Nuxt 內建獲取 API 資料的可組合項函式方法。

---

## 用法 Usage :

```html
<script setup lang="ts">
const { data, status, error, refresh, clear } = await useFetch('/api/modules',
  {
    /* 來自 $fetch 參數 */
    method:"POST",
    query: {},
    /* 攔截器參數 */
    onRequest({ request, options }) {},
    onRequestError({ request, options, error }) {},
    onResponse({ request, response, options }) {},
    onResponseError({ request, response, options }) {}

    /* 來自 useAsyncData 參數 */
    pick: [''],
    transform: ( response ) => { response } 
  }
)
</script>
```

### useFetch + Promise 同時請求

某些頁面會有多個 API 要發送，若一個一個執行，會造成阻塞，可以搭配 `Promise.all` 一次發送請求。

```js
// ❌ 阻塞
const { data: orgsData } = await useFetch(`https://api.github.com/orgs/nuxt`);
const { data: repoData } = await useFetch(`https://api.github.com/orgs/nuxt/repos`);

// ✅ 效率較高
const [{ data: orgsData }, { data: reposData }] = await Promise.all([
   useFetch(`https://api.github.com/orgs/nuxt`),
   useFetch(`https://api.github.com/orgs/nuxt/repos`),
]);
```


## 回傳值 Return Value :

- `data` : 非同步函式在發出請求後回傳的資料。
- `status` : 資料請求的狀態 ( `idle 尚未開始`、`peding 等待回應`、`success 請求成功`、`error 請求失敗` )。
- `error` : : 資料請求取得失敗，會回傳相關錯誤物件，成功時會回傳 null。
- `refresh` : 重新取得資料方法，用於更新資料。
- `clear` : 清除獲取 `data` 資料的方法，此時 `data` 將為 `null`，`error` 將為 `null`，`status` 將為 `idle`。

> [!NOTE]
> 當在 Vue 中的 `<script setup>` 環境調用 `data`、`status`、`error` 時應使用 `.value` 來存取。 而 `refresh`、`clear` 則為函式方法。

## 參數 Params :

- `URL` : 要獲取的 URL 獲 API 端
- `Options` : 可選物件 `{}`，可攜帶下列內容 :
  - `methods` : HTTP 請求方法 ( GET、POST、PUT、DELETE )
  - `query` : 查詢參數物件 `{}`，將參數透過 `?` 帶到 URL 上
  - `params` : query 的別名
  - `body` : Request body 請求時傳入的物件，將自動轉為字串
  - `headers` :  Request headers 請求時的標頭
  - `baseURL` : 基本的 API URL 路徑
  - `timeout` : 自動終止請求的毫秒數
  - `transform` : 是一個函式，用於修改 `handler` 的`請求結果`並回傳
  - `pick` : 為陣列格式，填入`屬性名稱`，從 `handler` 回傳的資料中取出指定屬性名稱資料。

## 攔截器 interceptors :

useFetch 攔截器是基於 [ofetch](https://github.com/unjs/ofetch#%EF%B8%8F-interceptors) 所使用，可用來處理請求過程中的設置。

```js
const { data } = await useFetch('/api/auth/login', {
  onRequest({ request, options }) {
    // 設置 request headers
    options.headers.set('Authorization', '...')
  },
  onRequestError({ request, options, error }) {
    // 處理 request 錯誤
  },
  onResponse({ request, response, options }) {
    // 處理回傳資料
     return response._data;
  },
  onResponseError({ request, response, options }) {
    // 處理 response 錯誤
  }
})
```