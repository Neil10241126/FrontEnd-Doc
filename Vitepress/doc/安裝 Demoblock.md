# 安裝 Demoblock

```bash
npm install -D vitepress-theme-demoblock
```

```json
"scripts": {
  "docs:dev": "yarn run register:components && vitepress dev docs",
  "docs:build": "yarn run register:components && vitepress build docs",
  "docs:serve": "vitepress serve docs",
  "register:components": "vitepress-rc"
}
```

#### Step1

開啟 vitepress 資料夾下的 `config.js` 檔案，套用 `demoblockPlugin` 和 `demoblockVitePlugin` 插件


```js
// vitepress/config.js
import { demoblockPlugin, demoblockVitePlugin } from 'vitepress-theme-demoblock'

markdown: {
  config: (md) => {
    md.use(demoblockPlugin)
  }
},
vite: {
  plugins: [demoblockVitePlugin()]
}
```

#### Step2

在 theme 資料夾下建立 useComponents.js 來建立註冊 vue 元件的方法

```js
// vitepress/theme/useComponents.js
// 引入相關 vue component
import Button from '../../../components/Button.vue'
import ButtonDelete from '../../../components/ButtonDelete.vue'

// 引入 vitepress-theme-demoblock 的 Demo DemoBlock 元件
import Demo from 'vitepress-theme-demoblock/dist/client/components/Demo.vue'
import DemoBlock from 'vitepress-theme-demoblock/dist/client/components/DemoBlock.vue'

export function useComponents(app) {
  app.component('XlButton', Button)
  app.component('ButtonDelete', ButtonDelete)

  app.component('Demo', Demo)
  app.component('DemoBlock', DemoBlock)
}

```


#### Step3

開啟 vitepress/theme 資料夾下 index.js 套用 itepress-theme-demoblock 主题 css 樣式

調用 useComponents 函式至 enhanceApp 下並傳入整個 app 物件來註冊元件跟功能

```js
import DefaultTheme from 'vitepress/theme'
// 下方這兩行
import 'vitepress-theme-demoblock/dist/theme/styles/index.css'
import { useComponents } from './useComponents'

export default {
  ...DefaultTheme,
  // 下方這行
  enhanceApp(ctx) {
    DefaultTheme.enhanceApp(ctx)
    useComponents(ctx.app)
  }
}
```

#### Step4

跟目錄建立 vue-component 資料夾，存放相關元件檔案

建立 Button.vue 元件

```html
<template>
  <button type="button" class="btn" :class="category">
    <slot/>
  </button>
</template>

<script>
export default {
  name: 'ButtonDelete',
  props: ['category']
}
</script>

<style scoped>
.btn {
  width: 100px;
  height: 40px;
  font-size: 16px;
  border: 1px solid #000;
}

.btn-primary {
  background-color: blue;
}
</style>
```



