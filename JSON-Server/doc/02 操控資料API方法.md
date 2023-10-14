# 02 操控資料 API 方法
---

##　資料範例

```json
{
  "products": [
    {
      "name": "A產品",
      "price":500,
      "language":{
        "zh-tw":"A產品",
        "en-us":"A product"
      },
      "id": 1
    },
    {
        "name": "B產品",
        "price":100,
        "language":{
          "zh-tw":"B產品",
          "en-us":"B product"
        },
        "id": 2
      }
      ,
      {
          "name": "C產品",
          "price":900,
          "language":{
            "zh-tw":"C產品",
            "en-us":"C product"
          },
          "id": 3
        }
  ]
}
```

## Json Server 包含以下 HTTP 請求方法及 API 

- **`Get` : 取得**
- **`Post` : 上傳**
- **`Put` : 修改**
- **`Delete` : 刪除** 

### 篩選 Filter

篩選資料使用 **`?`**，若有複數項目要篩選則增加 **`&`**來鏈接，
內層資料屬性則需要用 **`.`** 來選取。

```http
GET /products?name=A產品&price=500
GET /products?id=1&id=2
GET /products?language.zh-tw=B產品
```

### 關鍵字搜尋 Full-text search

使用 **`q`** 這個關鍵字可以針對特定資料進行搜索。

```http
GET /products?q=C產品
```

### 分頁 Paginate

使用 **`_page`** 及 **`_limit`** 來控制分頁及顯示幾筆資料，可用來設計每頁需顯示幾筆資料。

範例 : [顯示第1頁資料然後每頁顯示10筆。]()

```http
GET /products?_page=1&_limit=10
```

### 排序 Sort

使用 **`_sort`** 選擇排序規則， **`_order`** 控制升降冪排序。
**`_order`**後方接 **`asc`** 升冪、**`desc`** 降冪。

```http
GET /products?_sort=price&_order=asc
```

複選資料排序可以參考下方範例 : [(*代補充)]()

```http
GET /products?_sort=price,views&_order=desc,asc
```

### 區間範圍 Operators [(*代補充)]()

使用 **` _gte `** 大於 或 **` _lte `** 小於來設定範圍。

```http
GET /products?price_gte=10&price_lte=20
```

使用 **` _ne `** 來排除某個值，可搭配 **` & `** 來增排除項目。

```http
GET /products?id_ne=1
```

使用 **` _like `** 來篩選相符的值。(支援正規表達示)

```http
GET /products?name_like=A產品
```