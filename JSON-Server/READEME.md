### JSON-Server 使用筆記
---
[Github 說明](https://github.com/typicode/json-server)

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