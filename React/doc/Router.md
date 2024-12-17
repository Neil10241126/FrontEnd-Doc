# Router

## 安裝 react-router-dom

```bash
npm i -S react-router-dom
```

## 資料夾結構

```
src/
├── layouts/
│   ├── FrontLayout.tsx    # 前台 Layout
│   └── AdminLayout.tsx    # 後台 Layout
├── pages/
│   ├── front/             # 前台頁面
│   │   ├── Index.tsx
│   │   └── About.tsx
│   └── admin/             # 後台頁面
│       ├── Home.tsx
│       └── :User.tsx
└── routes/
    ├── index.tsx          # 主路由配置
    ├── frontRoutes.tsx    # 前台路由
    └── adminRoutes.tsx    # 後台路由
```

## 引入 Router 相關組件


在 `App.tsx` 頁面中從 `react-router-dom` 取出 `HashRouter` 元件，它會在整個 <App/> 元件的最外圍，讓導航以前端路由方式來切換。

```js
// ~/src/main.tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.tsx'
// 引入相關路由元件
import { HashRouter, Routes, Route } from 'react-router-dom'


createRoot(document.getElementById('root')!).render(
  <StrictMode>
    {/* 用 Router 元件包裹 App */}
    <HashRouter>
      <Routes>
        <Route path="*" element={ <App /> }></Route>
      </Routes>
    </HashRouter>
  </StrictMode>,
)
```


`<Routes/>` 元件類似 vue-router 的 `<router-view>`，它會在 `FrontLayout` 和 `AdminLayout` 元件中，根據路由的不同來切換顯示元件。

`<Route/>` 則是路由配置，類似 vue-router 中的`路由表 routes`，只不過配置方式是透過屬性 `path`、`element` 來定義要渲染的組件

> [!NOTE]
>	`<Route/>` 必須包裹在 `<Routes/>` 下才可使用，否則會出現錯誤！

```js
// ~src/App.tsx
import { Routes, Route, NavLink } from 'react-router-dom'

// 前台
import FrontLayout from './layouts/FrontLayout'
import Index from './pages/front/Index'
import About from './pages/front/About'

// 後台
import AdminLayout from './layouts/AdminLayout'
import Home from './pages/dashboard/Home'
import User from './pages/dashboard/User'


function App() {
  return (
    <>
      {/* 定義內部路由 */}
			<Routes>
				{/* 前台 FrontLayout */}
				<Route path='/' element={ <FrontLayout/> }>
					<Route index element={ <Index/> }></Route>
					<Route path="about" element={ <About/> }></Route>
				</Route>

				{/* 後台 AdminLayout */}
				<Route path="/admin" element={ <AdminLayout/> }>
					<Route index element={ <Home/> }></Route>
					<Route path=":user" element={ <User/> }></Route>
				</Route>
			</Routes>
    </>
  )
}

export default App
```

`Layout` 樣板處理方式使用 `<Outlet />` 元件來渲染子元件內容。

```js
// ~src/layouts/FrontLayout.tsx
import { Outlet } from "react-router-dom"

export default function FrontLayout() {
	return (
		<>
			<Outlet/>
		</>
	)
}

// ~src/layouts/AdminLayout.tsx
import { Outlet } from "react-router-dom"

export default function AdminLayout() {
	return (
		<>
			<Outlet/>
		</>
	)
}
```