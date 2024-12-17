# NavLink

#### 使用方式

當導航至特定頁面，需要為導航按鈕增添樣式時，可以使用 `<NavLink />` 組件，它會自動添加 `active` 的 class 樣式。

```js
import { NavLink } from "react-router-dom";

export default function Navbar() {
  return (
    <nav>
      <NavLink to="/" className="nav-link" >Home</NavLink>
      <NavLink to="/about" className="nav-link" >About</NavLink>
      <NavLink to="/faq" className="nav-link" >FAQ</NavLink>
    </nav>
  );
}
```

#### Props

在 `className`、`style`、`children` 上具有專門的 `callback props`，可用於條件渲染樣式。

```js
<NavLink
  to="/" 
  className={ ({ isActive }) => isActive ? 'isActive' : null }
  style={({ isActive }) => ({
    color: isActive ? 'red' : 'black',
  })} 
>Home</NavLink>
```


#### 自訂 class 名稱

`<NavLink />` 組件預設是呈現 `active` ，如果要自定義 class 名稱，需要使用 `jsx` 並用函式來帶入 `{ isActive }` 物件，並回傳自定義的 class 名稱，下方範例會回傳 `onActive` 。

```js
<NavLink
  to="/" 
  className={ ({ isActive }) => isActive ? 'onActive' : null } 
>Home</NavLink>
```

#### 合併原本的 className

如果原本就定義了 className 屬性了，這時出現兩個同樣屬性會發生錯誤 `warning: Duplicate key "className" in object literal`。

如果要解決，則改成下方使用`陣列`來合併，並使用 `join(' ')` 回傳合併後的字串

```js
<NavLink
  to="/" 
  className={ ({ isActive }) => [
    'nav-link border p-3',
    isActive ? 'onActive' : null
  ].join(' ') } 
>Home</NavLink>
```