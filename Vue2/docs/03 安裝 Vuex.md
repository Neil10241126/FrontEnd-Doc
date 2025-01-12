# 安裝 Vuex

```bash
npm install vuex@3
```

在 src 下新增 store 資料夾，並新增 index.js 檔案

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    count(state) {
      return state.count
    }
  },
  mutations: {
    addCount(state, num) {
      state.count += num
    }
  },
  actions: {}
})
```


在 main.js 中註冊 store

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router/index.js'
// 新增這行
import store from './stores/index.js'

new Vue({
  router,
  // 新增這行
  store,
  render: h => h(App)
}).$mount('#app')
```

修改 HomePage.vue 嘗試調用 Vuex 功能

```html
<template>
  <div>
    <h1>Home page</h1>
    <div>{{ count }}</div>
    <button type="button" @click="addCount">+1</button>
  </div>
</template>

<script>
export default {
  computed: {
    count() {
      return this.$store.getters.count;
    },
  },
  methods: {
    addCount() {
      this.$store.commit("addCount", 1);
    },
  },
};
</script>
```