# Vite Vue3 TS ESLint_Airbnb 專案
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
npm install -D eslint eslint-plugin-vue vite-plugin-eslint @vue/eslint-config-typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### 專案根目錄下新增 .eslintrc.cjs 檔案

**`.eslintrc.cjs`** 檔案中貼上以下基礎配置。

```js
// .eslintrc.cjs
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

開啟 **`vite.config.ts`** 載入 **`vite-plugin-eslint`** 模組並套用，並將 **`cache`** 設為 **`false`**，就可以在儲存時自動檢查調整你的格式

```ts
// vite.config.ts
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

[@vue/eslint-config-airbnb](https://www.npmjs.com/package/@vue/eslint-config-airbnb)：輸入以下命令安裝 Airbnb 規則。

```bash
npm install -D @vue/eslint-config-airbnb
```

找到 **`.eslintrc.cjs`** 檔案中的 **`extends`** 並套用 Airbnb 模組。

```js
// .eslintrc.cjs
extends: [
  'eslint:recommended',
  '@vue/typescript/recommended',
  'plugin:vue/vue3-essential',
  '@vue/eslint-config-airbnb'  // 新增這行
],
```

## 解決 vite.config.ts 錯誤

開啟 **`.eslintrc.cjs`** 檔案，並在底下新增一段語法告訴 ESLint 這三個套件可以被安裝在 **`devDependencies`** 中。

```js
// .eslintrc.cjs
module.exports = {
  // ... 以上省略。
  // 新增 settings 這行並將 Vite 套用的模組名稱打上。
  settings: {
    'import/core-modules': [
      'vite',
      '@vitejs/plugin-vue',
      'vite-plugin-eslint',
    ],
  },
};
```

## 安裝 eslint-config-airbnb-with-typescript

**_[[連結]](https://www.npmjs.com/package/@vue/eslint-config-airbnb-with-typescript)_**

此套件可以為 **`TS`** 專案中使用路徑別名 **`@`** ，讓 **`ESLint`** 解析路徑規則。

```bash
npm i -D @vue/eslint-config-airbnb-with-typescript
```

為 `.eslintrc.cjs` 套用模組。

```js
// .eslintrc.cjs
module.exports = {
  root: true,
  extens: [
    'eslint:recommended',
    '@vue/typescript/recommended',
    'plugin:vue/vue3-essential',
    '@vue/eslint-config-airbnb',
    '@vue/eslint-config-airbnb-with-typescript', // 新增這行
  ]
}
```

打開 **`tsconfig.json`** 檔案，增加以下語法告訴 TS 路徑規則。

```js
// tsconfig.json
{
  "compilerOptions": {
    "target": "ESNext",
    // ... 以上省略
    // 新增 paths 這行。
    "paths": {
      "@/*": ["./src/*", "./dist/*"]
    }
  },
}
```

打開 **`vite.config.ts`** 引入 **`path`** 模組，並告訴 **`Vite`** 編譯 dist 資料夾時解析 **`@`** 這個路徑，否則編譯時將出錯。

```js
// vite.config.ts
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import eslintPlugin from 'vite-plugin-eslint';
import path from 'path';  // 新增這行。

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    eslintPlugin({ cache: false }),
  ],
  // 新增 resolve 及 alias 內容。
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
});
```
> [!TIP]
> Helpful advice for doing things better or more easily.