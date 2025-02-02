# TypeScrpte 學習筆記
---

**專寫日期 : 2024/12/06**

**相關連結 : [官網 TypeScript 安裝說明](https://www.typescriptlang.org/download/)**

## 安裝 TypeScript

輸入命令將 ts 安裝於專案環境。

```base
npm install typescript --save-dev
```

查詢 ts 版本並確認是否安裝。

```base
npx tsc -v
```

查詢 ts 指令目錄 **`(確認 typescript 可以操作的指令)`**。

```base
npx tsc -help
```

## 建立第一個 TypeScript 檔案 

創建一個 **`ts_use`** 📁 資料夾並建立 **`index.ts`** 📄 檔案。

## 編譯 TypeScript 檔案 

> 編譯後會額外產生同樣檔名的 JavaScript 檔案。

### 全部編譯

執行以下命令將全部 `.ts` 檔案進行編譯。

```
npx tsc
```

### 單一檔案編譯

編譯方式為 `npx tsc` 命令加上 `.ts` 字尾的檔案名稱。


```base
npx tsc index.ts
```

> [!WARNING]
> 編譯單一檔案會自動忽略 tsconfig.json 設定，若需維持設定，請使用 npx tsc 指令來編譯，詳細可參考 [stack overflow](https://stackoverflow.com/questions/64934660/how-can-i-set-the-default-target-es-version-for-the-typescript-compiler-tsc)

## tsconfig.json 設定檔 

**_[TSConfig 配置設定](https://aka.ms/tsconfig)_** (設定檔屬性的詳細介紹)。

輸入以下指令來初始化 tsconfig 檔案。

```base
npx tsc --init
```

TSconfig 常用的相關設定 :

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