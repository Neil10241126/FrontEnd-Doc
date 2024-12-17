# 動態路由 Dynamic Routers

#### 單段動態路由

動態路由定義方式以 `:` 做開頭。

```js
<Routes>
  <Route path="/user" element={<UserLayout />}>
    <Route path=":userId" element={<User />} />
  </Route>
</Routes>
```

在動態路由頁面下，可以使用 `useParams` 解析當前路由參數給其他 API 應用。

```js
// ~/src/pages/User.tsx
import { useParams } from "react-router-dom";

export default function User() {
  let params = useParams();

  // 此時可解析 params 物件中的 userId
  console.log(params.userId)
}
```

#### 多段動態路由

當一個路由需要包含多段動態參數時，可以在同個路由路徑下使用 `/` 來串接多個動態參數 `:`。

```js
<Routes>
  <Route path="/user/:userId/post/:postId" element={<Post />} />
</Routes>
```

此時的多段動態參數皆可以獲取

```js
// ~/src/pages/Post.tsx
import { useParams } from "react-router-dom";

export default function Post() {
  let { userId, postId } = useParams();
  // ...
}
```


#### 可選路由

當路由參數未必存在時，可在路徑中使用 `?` 來表示該路由是`可選的`，下方範例意味著該路由可以匹配 `/:lang/categories` 或 `/categories` 兩種情況。

```js
<Routes>
  <Route path=":lang?/categories" element={<Categories />} />
</Routes>
```