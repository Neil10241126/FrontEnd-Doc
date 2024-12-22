# 巢狀路由 Nested Routers

---

**專寫日期 : 2024/12/19**

**相關連結 : []()**

---

路由需要延伸時運作方式為使用 `<Route />` 標籤包裹 `<Route />` 來進行嵌套。

```js
<Routes>
  <Route path="/dashboard" element={<Dashboard />}>
    <Route index element={<Home />} />
    <Route path="settings" element={<Settings />} />
  </Route>
</Routes>
```

此時位於父層的 `/dashboard` 路由下會包含 `/dashboard/settings` 的子層路由。


父層元件要呈現子層元件內容時使用 `<Outlet />` 元件來顯示。

```js
import { Outlet } from "react-router-dom";

export default function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      {/* 此時會顯示 <Home/> 或 <Settings/> 元件 */}
      <Outlet />
    </div>
  );
}
```