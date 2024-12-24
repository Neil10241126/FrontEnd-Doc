# Redux
---

**專寫日期 : 20--/--/--**

**相關連結 : []()**

---

##### 目錄
- [資料夾結構](#資料夾結構)
- [安裝套件](#安裝套件)
- [](#)
- [](#)

---

## 資料夾結構

```
src/
├── assets/
├── components/
├── pages/
├── HOC/
├── routes/
└── store/
    ├── store.js           # 設定 Redux store 的檔案，負責整合所有的 reducer。
    ├── counterSlice.js    # 定義 counter slice，包含狀態、reducers 和 actions。
    └── anotherSlice.jsx   # 定義其他 slice
```

## 安裝套件

#### Step1: 引入

```bash
npm install react-redux @reduxjs/toolkit
```

#### Step2: 定義 Slice

```js
import { createSlice } from "@reduxjs/toolkit";

export const counterSlice = createSlice({
  // slice 名稱
  name: 'counter',
  // 資料
  initialState: {
    value: 0,
  },
  // 方法
  reducers: {
    incre: (state) => {
      state.value += 1;
    },
    decre: (state) => {
      state.value -= 1;
    }
  }
})

// 透過 actions 取得方法，並 export 至其他檔案使用
export const { incre, decre, } = counterSlice.actions;

// 透過 reducer 取得狀態，並 export 至 store.js 中套用
export default counterSlice.reducer;
```

##### Props :

- `name` : 為 slice 名稱，用來識別這個 slice，該名稱會用於生成 action type 的前綴，確保唯一性。例如 `name` 為 `counter` ，則生成 action type 為 c`ounter/increment`、`counter/decrement`。
- `initialState` : 物件結構 `{}` ，內部放入需要保存的`資料`及`狀態`
- `reducers` : 物件結構 `{}`，定義操控 slice 的`方法`。

#### Step3: 定義 Store 並套用

使用 `configureStore` 定義 store，並將 slice 檔案套用至 `reducer` 中。

```js
// ~/src/store/store.js
import { configureStore } from "@reduxjs/toolkit";
// import your slice
import counterSlice from './counterSlice'

export const store = configureStore({
  reducer: {
    counterSlice,
    // other slice...
  }
})
```

#### Step4: 

回到 `main.jsx` 中，使用 `Provider` 元件包裹整個 `<App/>` 來套用 store。

```js
// ~/src/main.jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { HashRouter } from 'react-router-dom'
import Router from './routes/index.tsx'
// 使用 <Provider/> 並套用 store
import { Provider } from 'react-redux'
import { store } from './store/store'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <Provider store={store}>
      <HashRouter>
        <Router />
      </HashRouter>
    </Provider>
  </StrictMode>,
)
```

#### Step5: 元件引入 Redux Store

要使用 slice 的檔案引入 `useSelector` 和 `useDispatch` 來取得資料狀態和方法。

- `useSelector` : 用於從 Redux store 中提取資料狀態。它接收一個`函數`作為參數，透過內部的參數來取得 Redux Store 的 slice。
- `useDispatch` : 為一個返回函式，該函式傳入要執行的方法。


```js
// ~/src/App.jsx
import { useSelector, useDispatch } from 'react-redux';
import { incre, decre } from './store/counterSlice'

function App() {
  const count = useSelector((state) => state.counterSlice.value);
  const dispatch = useDispatch();

  return (
    <>
      <h1>Counter: {count}</h1>
      <button onClick={() => dispatch(incre())}>Increment</button>
      <button onClick={() => dispatch(decre())}>Decrement</button>
    </>
  )
}

export default App
```