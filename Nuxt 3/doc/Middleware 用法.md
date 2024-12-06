# Middleware 用法
---

**專寫日期 : 2024/11/06**

**相關連結 : [官網 middleware](https://nuxt.com/docs/guide/directory-structure/middleware)**

---

# 什麼是 Middleware ?

`middleware` 是 `Nuxt` 提供的路由中間件，功能類似於 `vue-router` ，可在整個應用程式當中處理導航至特定路由前的處理。

middleware 主要分為三種 :

1. `匿名 middleware` : 直接在組件當中定義。
2. `具名 middleware` : 存放於 middleware/ 資料夾下，組件中自行選擇調用。
3. `全域 middleware` : 存放於 `middleware/` 資料夾下且檔名帶有 .global 中間名，每次路由變更時運行。

📌優先順序 `全域 > 具名`，其他關於執行序補充 : [Ordering Global Middleware](https://nuxt.com/docs/guide/directory-structure/middleware#ordering-global-middleware)

# 常見使用場景