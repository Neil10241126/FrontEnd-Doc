# 02 Type 與 Interface
---

##### 目錄
- [Type 型別別名](#type-型別別名)
  - [【基礎】Type 定義](#基礎type-定義)
  - [【進階】Type 擴充](#進階type-擴充)
- [Interface 介面](#interface-介面)
  - [【基礎】Interface 定義](#基礎interface-定義)
  - [【進階】Interface 擴充](#進階interface-擴充)
  - [【進階】Interface 合併](#進階interface-合併)
  - [【進階】declare module 擴充外部 Interface](#進階declare-module-擴充外部-interface)


## Type 型別別名

### 【基礎】Type 定義

使用 *`type`* 關鍵字加上 *`型別別名`* 搭配 *`=`* 來定義資料。

> 型別別命 : 自定義名稱來代表型別，使用大駝峰命名。

```ts
// 對變數定義。
type LiveName = string | boolean | number;
let data: LiveName;

// 對陣列定義。
type Array = (string | number)[];
let array: Array;

// 對 Tuple 定義。
type Tuple = [string, number, boolean];
let tuple: Tuple;

// 對物件進行定義。
type Obj = { name: string, age: number};
let obj: Obj;
```

### 【進階】Type 擴充

擴充概念為繼承原本物件特性來額外拓展資料內容，並且不會影響原本物件，來提高物件使用的靈活性。

**`擴充情境`** : 如果門是一個類別，車是一個類別，但兩個都需要防盜系統變成防盜門跟防盜車，這時候防盜系統就會是這兩個的子類別，就可以額外實現警報功能來擴充至這兩個類別之中。

使用 *`&`* 加上 *`{ 擴充屬性: 類型 }`* 來擴充資料。

```js
// 先定義警報類別。
type Alarm  = { alarmMsg: string; };
// 將警報系統定義至門類別，並額外擴充屬性。
type Door = Alarm & { doorName: string; };
// 將警報系統定義至車類別。
type Car = Alarm;

let door: Door = {
  alarmMsg: '警報訊息',
  doorName: '警報門',
};

let car: Car = {
  alarmMsg: '警報訊息',
}
```

## Interface 介面

### 【基礎】Interface 定義

使用 *`interface`* 關鍵字加上 *`型別別名`* 搭配 *`{}`* 來定義資料。

> Interface 僅能定義 **物件類型** 的資料。

```js
// 對物件進行定義。
interface UserCard {
  name: string,
  desc: string,
  age?: number,
};

const card: UserCard = {
  name: 'Neil',
  desc: '卡片描述',
  // age 為可選屬性，可加可不加。
};
```

### 【進階】Interface 擴充

Interface 擴充概念類似於 Type ，但仍有些許不同，Interface 屬於繼承概念。

使用 *`extends`* 關鍵字加上 *`型別別名 { 擴充屬性: 類型 }`* 來擴充資料。

```ts
// 先定義警報介面。
interface Alarm {
  alarmMsg: string
}
// 定義門介面，並繼承警報介面的內容，在額外擴充屬性。
interface Door extends Alarm {
  doorName: string;
}

let door: Door = {
  alarmMsg: '警報訊息',
  doorName: '警報門'
}
```

### 【進階】Interface 合併

相同的 Interface 名稱在 TS 當中會進行`合併`，進一步擴充所有類型，這是 type 所沒有的。

情境範例：目前有一個廠商的 interface 會包含基本資料，包含廠商名稱、地址及電話，而某個 page 下會需要額外擴充廠商販售的商品資訊，因此需要將兩個 interface 合併。

```ts
// 定義 廠商 的基本 interface
interface Vendors {
  vendorsName: string,
  address: string,
  tel: string,
}

// 增加 廠商的 interface 擴充商品資訊
interface Vendors {
  products: Array<{
    productName: string,
    price: number,
    quantity: number,
  }>
}

// 定義資料
const vendorExample: Vendors = {
  vendorsName: '',
  address: '桃園',
  tel: '0123456789',
  products: [
    { productName: '商品Ａ', price: 100,quantity: 10, },
    { productName: '商品B', price: 200,quantity: 15, },
  ],
}
```

### 【進階】declare module 擴充外部 Interface

`declare module “模組名稱或路徑” {}` 的語法，可以擴充外部模組的 interface 或 type，下方範例引入外部 `Vendors` interface 來進一步合併類型。

```ts
// interface.ts
export interface Vendors {
  vendorsName: string,
  address: string,
  tel: string,
}
```

```ts
// index.ts
import { Vendors } from "./interface";

declare module "./interface" {
  interface Vendors {
      products: Array<{
        productName: string,
        price: number,
        quantity: number,
      }>
  }
}
```