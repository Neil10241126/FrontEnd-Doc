# useEffect
---

**專寫日期 : 2024/12/22-**

**相關連結 : [React官網 - useEffect](https://zh-hans.react.dev/reference/react/useEffect)**

---

##### 目錄
- [使用方式](#使用方式)
- [參數](#參數)
- [useEffect 生命週期](#useeffect-生命週期)
- [使用 useState 注意事項](#使用-useeffect-注意事項)

---

## 使用方式

```jsx
useEffect(setup, dependencies?)
```

## 參數

- `setup函式` : 為 `callback函式`，定義觸發時要執行的程式碼，依賴於 `dependencies` 變化時會重新執行此函式。
- `dependencies` : 表示觸發 `useEffect` 的條件，為 `[]` 結構，內部帶入響應式值包括 props、state 等，實際範例為 `[dep1, dep2, dep3, ...]`。

## useEffect 生命週期

useEffect 生命週期依賴於 dependencies 定義方式會影響運作方式，主要分為三種情況：


##### 1. 無 dependencies

若無定義 dependencies，則初始化及元件更新就都會執行程式。

```jsx
useEffect(() => {
  // 初始化 ＋ 元件更新都會執行
  console.log('useEffect run!')
})
```

##### 2. 空陣列 [] dependencies

定義為 `[]` 時，僅元件初始化時會執行，類似於 Vue 的 `mounted`。

```jsx
useEffect(() => {
  // 初始化執行
  console.log('useEffect run!')
}, [])
```


##### 3. 有值的 dependencies

用於追蹤特定 `state` 或 `props` 變化時執行特定行為，類似於 Vue 的 `watch`。

```jsx
useEffect(() => {
  // dependencies 值變化時才執行
  console.log('useEffect run!')
}, [count])
```

## 使用 useEffect 注意事項

