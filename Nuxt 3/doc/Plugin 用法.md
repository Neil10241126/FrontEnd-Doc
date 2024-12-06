# Plugin 用法
---

**專寫日期 : 2024/11/06**

**相關連結 : 無**

---

# 什麼是 Nuxt Plugin ?

Plugins 是在 Nuxt 應用程式啟動時自動執行的功能模組，主要用於：

1. 註冊全局組件
2. 注入功能或變數
3. 添加 Vue 指令
4. 整合第三方套件

---

# 常見使用場景

## 依賴注入

`plugin` 可以用於 `注入依賴`，可以將一些簡單的邏輯放在  `provide: {}` 當中，並 `return` 出去，這樣 `全域`都可以使用這些方法。

Step1 : 直接 return 並包裹 provide 物件，將方法寫在裏頭。

```js
// 'plugins/hello.js'
export default defineNuxtPlugin((nuxtApp) => {
  return {
    provide: {
      hello: (msg) => `hello ${msg}`,
      goodbye: (name) => `goodbye ${name}`,
      currentTime: () => new Date().toLocaleTimeString(),
    }
  }
})
```

Step2 : 調用時從 `nuxtApp()` 中解構取出，即可在組件中使用。

```html
<script setup>
const { $hello, $goodbye, $currentTime } = useNuxtApp();
</script>

<template>
  <p>{{ $hello('Mike') }}</p>
  <p>{{ $goodbye('Mike') }}</p>
  <p>Current time: {{ $currentTime() }}</p>
</template>
```

## 自定義指令 ( v-directive )

當遇到時間轉換的情境，我們可以使用 `plugin` 搭配 `v-directive` 自定義指令方式來處理時間戳記的格式。

Step1 : 安裝 [dayjs](https://day.js.org/en/) 插件並引入，調用 `nuxtApp.vueApp.directive` 方法

```js
// 'plugins/timeformate.js'
import dayjs from "dayjs"

export default defineNuxtPlugin((nuxtApp) => {
  // directive('指令名稱', 生命週期物件{})
  nuxtApp.vueApp.directive('timeformate', {
    // el : 指令綁定的元素
    // binding : 指令綁定的值
    mounted: (el, binding) => {
      const time = dayjs(binding.value).format('YYYY-MM-DD');
      el.innerHTML = `此處顯示時間 : ${time}`;
    }
  })
})
```

Step2 : 套用的元素加上 `v-[指令名稱]="[變數值]"` 即可

```html
<template>
  <h3 v-timeformate="1730863912222">此處顯示時間 : </h3>
</template>
```

📌 關於 directive 相關說明，請參閱官網 : [自定義指令](https://cn.vuejs.org/guide/reusability/custom-directives.html)

## 第三方套件整合

有時候相關插件沒有 Nuxt 版本時，`plugin` 可以協助整合，用法類似原本 Vue 使用 `app.use()` 形式，這裡將以 [VCalendar](https://vcalendar.io/) 作為範例。

Step1 : 引入欲整合的插件，並調用 `nuxtApp.vueApp.use` 方法將插件引入

```js
// 'plugins/calendar.js'
import VCalendar from 'v-calendar';
import 'v-calendar/style.css';

export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.vueApp.use(VCalendar);
})
```

Step2 : 直接在 `.vue` 檔案中即可使用。

```html
<script setup>
const selectedDate = ref(new Date())
</script>

<template>
  <VDatePicker v-model='selectedDate' />
</template>
```

---

# Plugins 使用注意事項

## 中間名 `.client.js` 和 `.server.js`

當使用具備 `client` 和 `server` 的插件時，Nuxt 會自動根據環境載入對應的插件，如果不加上中間名，則是兩端都執行。

## `<ClientOnly>`

> [!WARNING]
> 組件使用到 `client` 插件時，必須包裹 `<ClientOnly>` 才不會抱錯。 

```js
// 'plugins/hello.client.js'
export default defineNuxtPlugin((nuxtApp) => {
  return {
    provide: {
      hello: (msg) => `hello ${msg}`,
    }
  }
})
```


```html
<script setup>
const { $hello } = useNuxtApp();
</script>

<template>
  <ClientOnly>
    <p>{{ $hello('Mike') }}</p>
  </ClientOnly>
</template>
```