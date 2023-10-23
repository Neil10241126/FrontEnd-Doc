# Vite Vue3 TS Eslint_Airbnb 專案
---

## 安裝 Vite、Vue 及 TypeScript

使用終端命令創建一個 Vite 專案，並選擇 **`4.0.0`** 版本，不要使用 **`latest`** 版。

```bash
npm create vite@4.0.0
```

專案選擇 Vue.js 及 使用 TypeScript。(代補充...)

## 安裝 ESLint

- [eslint](https://www.npmjs.com/package/eslint) : ESLint 主要核心包，一定要安裝。
- [eslint-plugin-vue](https://eslint.vuejs.org/) : Vue.js 的官方 ESlint 插件，可以檢查 Vue 的程式碼。
- [vite-plugin-eslint](https://www.npmjs.com/package/vite-plugin-eslint) : Vite 使用的 ESLint 插件，幫助瀏覽器看到 ESLint 警告畫面。
- [@vue/eslint-config-typescript](https://www.npmjs.com/package/@vue/eslint-config-typescript) : 用於 Vue 的 eslint config typescript。
- [@typescript-esliint/eslint-plugin](https://www.npmjs.com/package/@typescript-eslint/eslint-plugin) : 可以替 TypeScript 提供 lint 規則的插件。
- [@typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser) : TypeScript 的 eslint 解析器。

```bash
npm i -D eslint eslint-plugin-vue vite-plugin-eslint @vue/eslint-config-typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### 根目錄下新增 .eslintrc.cjs 檔案

放上基礎配置。

```cjs
module.exports = {
  root: true,
  env: {
    node: true,
    es2021: true
  },
  extends: [
    "eslint:recommended",
    "@vue/typescript/recommended",
    "plugin:vue/vue3-essential",
  ],
  parserOptions: {
    parser: '@typescript-eslint/parser',
    ecmaVersion: 2020,
  },
  rules: {
    // 這邊可以自訂規則
  },
}
```

### 套用 vite-plugin-eslint 模組

開啟 `vite.config.ts` 載入 `vite-plugin-eslint` 模組並套用，並將 `cache` 設為 `false`，就可以在儲存時自動檢查調整你的格式

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import eslintPlugin from "vite-plugin-eslint" // 新增這行

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    eslintPlugin({ cache: false }) // 新增這行
  ]
})
```

## 安裝 ESLint Airbnb 規範

[@vue/eslint-config-airbnb](https://www.npmjs.com/package/@vue/eslint-config-airbnb)：在 Vue 裡面使用 standard 規範

```bash
npm install -D @vue/eslint-config-airbnb
```

為 .eslintrc.cjs 套用 Airbnb 規範。

```cjs
extends: [
  'eslint:recommended',
  '@vue/typescript/recommended',
  'plugin:vue/vue3-essential',
  '@vue/eslint-config-airbnb'  // 新增這行
],
```

## 安裝 eslint-config-airbnb-with-typescript