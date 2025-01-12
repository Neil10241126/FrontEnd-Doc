# 安裝 Vuetify2

```bash
npm install vuetify@2
npm install -D sass@1.32.0
npm install -D unplugin-vue-components
```

在 src 下新增 plugins 資料夾，新增 vuetify.js 檔案

```js
import Vue from "vue";
import Vuetify from "vuetify/lib";

Vue.use(Vuetify)

export default new Vuetify({})
```

在 main.js 中引入 vuetify

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router/index.js'
import store from './stores/index.js'
// 新增這行
import vuetify from './plugins/vuetify.js'

new Vue({
  router,
  store,
  // 新增這行
  vuetify,
  render: h => h(App)
}).$mount('#app')
```

修改 vite.config.js 引入

```js
import { createVuePlugin } from 'vite-plugin-vue2'
import { defineConfig } from 'vite'
// 添加下面兩行
import { VuetifyResolver } from 'unplugin-vue-components/resolvers'
import Components from 'unplugin-vue-components/vite'

export default defineConfig({
  plugins: [
    createVuePlugin(),
    // 添加下面
    Components({
      resolvers: [VuetifyResolver()],
      directoryAsNamespace: true
    }),
  ],
})

```