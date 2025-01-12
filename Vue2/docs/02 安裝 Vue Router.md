# 安裝 Vue Router

```bash
npm install vue-router@3
```

在 src 下新增 router 資料夾，然後建立 index.js 檔案，撰寫路由表

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

// 注册路由插件
Vue.use(VueRouter)

const routes = [
  {
   path: '/',
   name: 'Home',
   component: () => import('../views/HomePage.vue')
  },
  {
   path: '/about',
   name: 'About',
   component: () => import('../views/AboutPage.vue')
  }
]

const router = new VueRouter({
  routes
})

export default router
```

在 src 下新增 views 資料夾，然後分別建立 HomePage.vue 和 AboutPage.vue 檔案

```html
<template>
  <h1>Home page</h1>
</template>

<template>
  <h1>About page</h1>
</template>
```


然後在 main.js 中全域註冊 router 

```js
import Vue from 'vue'
import App from './App.vue'
// 新增這行
import router from './router/index.js'

new Vue({
  // 新增這行
  router,
  render: h => h(App)
}).$mount('#app')
```

修改 App.vue 檔案分別顯示兩個頁面

```html
<template>
  <div id="app">
    <RouterLink to="/">Home</RouterLink> |
    <RouterLink to="/about">About</RouterLink>
    <hr>
    <RouterView />
  </div>
</template>
```