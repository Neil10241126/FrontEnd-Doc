# TypeScrpte 學習筆記
---

## 安裝 TypeScript

輸入命令來進行 ts 全域安裝。

```base
npm install -g typescript
```

查詢 ts 版本並確認是否安裝。

```base
tsc -v
```

查詢 ts 指令目錄 **`(確認 typescript 可以操作的指令)`**。

```base
tsc -help
```

## 建立第一個 TypeScript 檔案 

創建一個 **`ts_use`** 📁 資料夾並建立 **`index.ts`** 📄 檔案。

## 編譯 TypeScript 檔案 

編譯方式為 `tsc` 命令加上 `.ts` 字尾的檔案名稱。

> 編譯後會額外產生同樣檔名的 JavaScript 檔案。

```base
tsc index.ts
```

## tsconfig.json 設定檔 

**_[TSConfig 官網](https://www.typescriptlang.org/tsconfig)_** (設定檔屬性的詳細介紹)。

輸入以下指令來初始化 tsconfig 檔案。

```base
tsc --init
```

TSconfig 重要的相關設定 :

```json
{
  "compilerOptions": {
     /* Language and Environment 語言與環境 */
     "target": "es2016", // 設定欲輸出的 JavaScript 版本。
     /* Modules 模組 */
      "module": "commonjs",  // 設定欲執行的模組化規範。
      "rootDir": "./src",   // 指定輸入的 .ts 檔案來源路徑。
      /* JavaScript Support 輔助功能 */
      "allowJs": true,  // 啟用時允許 TypeScript 引入 JS 檔案。
      /* Emit */
      "sourceMap": true, // 啟用後產生 .map 檔案，並在瀏覽器console端除錯時直接指向 .ts 檔案。
      "outDir": "./dist/",  // 指定輸出的路徑位置。
      /* Type Checking */
      "strict": true, // 啟用後執行嚴格類型 JavaScript 檢查選項 "use strict"  
      "strictNullChecks": true, // 啟用後類型檢查包含 null 及 undefined 類型。
  }
}
```