### JSON-Server 使用筆記
---
[_JSON-Server 官網_](https://github.com/typicode/json-server)

### 目錄

- [開始](#開始)
  - [01 資料格式設計](./doc/01%20資料格式設計.md)
  - [02 操控資料 API 方法](./doc/02%20操控資料API方法.md)
  - [03 關聯式資料庫](./doc/03%20關聯式資料庫.md)
  - [04 JSON-Server Auth 身分驗證](./doc/04%20JSON-Server%20Auth%20身分驗證.md)
  - [05 授權流程](./doc/05%20授權流程.md)

## 開始

安裝 JSON-Server

```
npm install -g json-server
```

創建一個 `db.json` 檔案來存放資料

```json
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

啟動 JSON Server

```bash
json-server --watch db.json
```

如果你看到 [http://localhost:3000](http://localhost:3000)，表示運行成功