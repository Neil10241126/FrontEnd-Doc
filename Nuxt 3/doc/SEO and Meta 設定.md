# SEO and Meta 設定
---

**專寫日期 : 2024/11/06**

**相關連結 : [SEO and Meta](https://nuxt.com/docs/getting-started/seo-meta)**

---

meta tag 三種設定方式 :

1. `全域 meta tag` : 包含整個應用程式共用設定。
2. `Nuxt 方法` : 單一頁面設定，包含 `useHead()`、`useSeoMeta`、`useServerSeoMeta`。
3. `SEO Component` : 提供 `<Title>`、`<Meta>`、`<Link>` 等元件，可直接使用。


# 如何使用

## `全域 meta tag`

在 `nuxt.config.js` 設定檔寫入相關預設值，可參考下方撰寫範例或[官網說明 seo-meta: default](https://nuxt.com/docs/getting-started/seo-meta#defaults)

> [!WARNING]
> 此方法屬性資料不具備響應性，建議在 `app.vue` 中使用 `useHead()`

```js
// 'nuxt.confing.js' 全域設定 meta tag
export default defineNuxtConfig({
  "app": {
    "head": {
      "viewport": "width=500, initial-scale=1",
      "title": "Nuxt3 高效入門全攻略",
      "meta": [
        { "name": "description", "content": "這是 Mike 的 Nuxt3 高效入門全攻略課程" },
        { "property": "og:title", "content": "Nuxt3 高效入門全攻略" },
        { "property": "og:url", "content": "http://localhost:3000/" },
        { "property": "og:description", "content": "這是 Mike 的 Nuxt3 高效入門全攻略課程" },
      ]
    }
  }, 
})
```

## `useHead()`

針對單一頁面的 `meta tag` 進行調整，可覆蓋全域的預設值，但此設定方式較為麻煩，建議使用 `useSeoMeta` 或 `useServerSeoMeta` 。

📌 更多 useHead() 補充說明，請參閱官網 : [useHead()](https://nuxt.com/docs/api/composables/use-head)

```html
<script setup>
useHead({
 title: "關於我們 About - Nuxt3 高效入門全攻略",
 meta: [
   { property: "og:title", content: "關於我們 About - Nuxt3 高效入門全攻略" },
   { property: "og:url", content: "http://localhost:3000/about" },
   { property: "og:image", content: "http://localhost:3000/share.jpg" },
   { name: "description", content: "關於我們 About - 最棒的Nuxt3的線上課程" },
   { property: "og:description", content: "關於我們 About - 最棒的Nuxt3的線上課程" },
 ],
});
</script>
```

## `useSeoMeta`

這是 Nuxt 提供處理 SEO 的新方法，提升撰寫 meta tag 效率，減少屬性錯誤。

📌 官網說明 : [useSeoMeta](https://nuxt.com/docs/api/composables/use-seo-meta)

```html
<script setup>
useSeoMeta({
 title: "About - Nuxt3 高效入門全攻略",
 description: "關於我們 - 最棒的Nuxt3的線上課程",
 ogDescription: "關於我們 - 最棒的Nuxt3的線上課程",
 ogTitle: "About - Nuxt3 高效入門全攻略",
 ogImage: 'https://example.com/image.png',
 // 其他屬性省略...  
});
</script>
```

## `useServerSeoMeta`

當 SEO 資訊不需要再 Client 端展示的時候，使用 useServerSeoMeta 比較有效率

📌 官網說明 : [useServerSeoMeta](https://nuxt.com/docs/api/composables/use-server-seo-meta)

```html
<script setup>
// 取得遠端 api 的 mate tag 資料
const res = await useFetch('https://vue-lessons-api.vercel.app/seo/about');

useServerSeoMeta({
  title: () => `${res.data.value.title} - Nuxt3`,
  ogTitle: () => `${res.data.value.title} - Nuxt3`,
  description: () => `${res.data.value.description} - Nuxt3`,
  ogDescription: () => `${res.data.value.description} - Nuxt3`,
  // 其他屬性省略... 
});
</script>
```

關於 `useSeoMeta`、`useServerSeoMeta` 比較

| 方法 | 執行端 | 響應式更新 meta | 適用場景 |
| ---  | --- | -- | -- |
| `useSeoMeta` | Client、Server | 可以響應式更新 meta 標籤 | 適用於 Client 需要動態更新 SEO 資訊的場景 |
| `useServerSeoMeta` | Server | 只在初始頁面載入預設 meta 標籤 | 適用於 SEO 資訊是靜態的或只依賴伺服器端資料的場景 |