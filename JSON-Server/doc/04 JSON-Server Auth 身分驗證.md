# 🔐JSON Server Auth 身分驗證
---

- _**[GitHub - JSON Server](https://github.com/typicode/json-server)**_
- _**[GitHub - JSON Server Auth](https://github.com/jeremyben/json-server-auth)**_

利用 JSON Server 及 JSON Server Auth 來設計虛假的 **身分驗證** 及 **授權流程** 。

## 開始

創建一個 **`json_server_auth`** 📁 資料夾

初始化專案 

```bash
npm init
```

安裝 `JSON Server` 和 `JSON Server Auth` : 開發或測試環境請加上 **`-D`**，若部屬伺服器則請移除前綴。

```bash
npm install -D json-server json-server-auth
```

建立一個 `db.json`📁 檔案並建立一個 **`users`** 集合

```json
{
  "users": []
}
```

以 JSON Server Auth 來開啟 JSON Server 伺服器

```bash
json-server-auth db.json
```

## 驗證流程 API🔑

JSON Server Auth 的身分驗證流程是基於簡單的 **[JWT base](https://jwt.io/)** 來添加。
用來 **`模擬虛假的身分驗證和授權流程`** 。

### 註冊功能 👥

以下三種路由皆能註冊一個新的帳戶

- **`POST -  /register`**
- **`POST -  /signup`**
- **`POST -  /users`**

POST 資料中需夾帶 **`email`** 和 **`password`** 兩個必要屬性。
> 可以額外增加其他屬性資料，且無須驗證，如果你有這個需求。

```http
POST /register
{
  "email": "olivier@mail.com",
  "password": "bestPassw0rd"
}
```

密碼會由 **[bcryptjs](https://github.com/dcodeIO/bcrypt.js)** 加密，包含 JWT access token ( 期限為1小時 )，和用戶數據 ( 不包含密碼 )。

```http
201 Created
{
  "accessToken": "xxx.xxx.xxx",
  "user": {
    "id": 1,
    "email": "olivier@mail.com"
  }
}
```

## 登入功能🪪

以下兩種路由皆能登入

- **`POST - /login`**
- **`POST -  /signin`**

POST 資料中需夾帶 **`email`** 和 **`password`** 兩個必要屬性。

```http
POST /login
{
  "email": "olivier@mail.com",
  "password": "bestPassw0rd"
}
```

成功後會回傳 200 狀態及用戶資料，包含 JWT access token ( 期限為1小時 )，和用戶數據 ( 不包含密碼 )。

```http
200 OK
{
  "accessToken": "xxx.xxx.xxx",
  "user": {
    "id": 1,
    "email": "olivier@mail.com",
  }
}
```

## 修改密碼🏷️ (無權限)

> 以下範例為示範修改流程，實際開發會帶入 **token** 來確認是否本人操作，並帶入 **狀態路由(400、600...)** 來確認權限。

路由指向須為 **`/users`** 且後方帶入本人 **`/id`** 。

- **`PATCH -  /users/1`**

PATCH 資料中需夾帶變更的 **`password`** 必要屬性。

```http
PATCH /users/1
{
  "password": "changePassw0rd"
}
```

成功後會回傳 200 狀態及用戶資料，包含密碼 (已加密)。

```http
200 OK
{
  "data": {
    "id": 1,
    "email": "olivier@mail.com",
    "password": "xxx.xxx.xxx"
  }
}
```

## 修改密碼🏷️ (有權限)

路由後方需增加狀態碼 **`600`** (關於[路由防護](./05%20授權流程.md)) 。

- **`PATCH -  /600/users/1`**

PATCH 資料中需夾帶變更的 **`password`** 必要屬性。
夾帶 **`headers`** 物件 {}，並加入屬性 **`Authorization`** ，帶入 **`token`** 來確認是否為本人。 

```http
PATCH /600/users/1
{
  {
    "password": "changePassw0rd"
  },
  {
    "headers": {
      "authorization": "Bearer ${token}"
    },
  }
}
```

成功後會回傳 200 狀態及用戶資料，包含密碼 (已加密)。

```http
200 OK
{
  "data": {
    "id": 1,
    "email": "olivier@mail.com",
    "password": "xxx.xxx.xxx"
  }
}
```