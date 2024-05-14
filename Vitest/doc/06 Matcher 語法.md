# 06 Matcher 比對語法

---

Matcher 用於檢驗結果的工具，比較常見的是使用 `.tobe()`，Matcher 可以檢查各種類型的條件，常見的類型可以分為:

- ###### [純值型別比對 (Primitive Types)](#純值型別比對-primitive-types)
- ###### [非純值型別比對 (Non Primitive Types)](#非純值型別比對-non-primitive-types)
- ###### [監聽與模擬](#監聽與模擬)
- ###### [Error](#error)
- ###### [快照](#快照)

## 🔗1.純值型別比對 (Primitive Types) :

主要用於純值型別，如 String、Number、Boolean、undefined、null、Symbol。

#### 1-1 比對值是否完全相等

**`toBe`** : 比對值是否完全相等。※ 注意物件或陣列型別比對的是記憶體位置，則會發生錯誤。

```js
expect(1).toBe(1)    // passed
expect('1').toBe(1)  // failed
expect({}).toBe({})  // failed 物件比對的是記憶體位置
```

#### 1-2 浮點數運算時的比對

**`toBeCloseTo`** : 專門處理浮點數相關比對，若使用 `toBe` 比對容易發生運算時小數點的值發生益位問題。

```js
expect(0.1 + 0.2).toBe(0.3) 
// falied 浮點數 issue，結果為 0.300000000000004

expect(0.1 + 0.2).toBeCloseTo(0.3) // passed
```

#### 1-3 檢查資料處理長度是否正確

**`toBeGreaterThan`** : 結果大於預期目標。
**`toBeGreaterThanOrEqual`** : 結果大於等於預期目標。
**`toBeLessThan`** : 結果小於預期目標。
**`toBeLessThanOrEqual`** : 結果小於等於預期目標。

> 可用於檢查 API 回傳資料長度是否正確，或是日期、時間上的處理。

```js
expect(5).toBeGreaterThan(1)        // passed
expect(5).toBeGreaterThanOrEqual(5) // passed
expect(6).toBeLessThan(7)           // passed
expect(6).toBeLessThanOrEqual(6)    // passed

```

#### 1-4 確保主程式是否正確載入

**`toBeDefined、toBeUndefined`** : 用於確認主程式是否正確載入的首要測試，若無順利載入，後續測試則沒任何意義。

```js
var a = ''
var b
expect(a).toBeDefined()    // passed
expect(b).toBeUndefined()  // passed
```

#### 1-5 想確認有值，但不在實際的值為何

**`toBeTruthy、toBeFalsy`** : 該 API 比較的並非是布林值 (true、false)，而是真值與假值 (truthy、falsy)，單純確認是否存在值的時候，可以使用。

```js
// toBeTruthy 真值比較
expect(1).toBeTruthy()         // passed
expect({}).toBeTruthy()        // passed
expect([]).toBeTruthy()        // passed

// toBeFalsy 假值比較
expect(0).toBeFalsy()          // passed
expect('').toBeFalsy()         // passed
expect(null).toBeFalsy()       // passed
expect(undefined).toBeFalsy()  // passed
expect(NaN).toBeFalsy()        // passed
```

#### 1-6 需要回傳 null 或應該是 null 值使用

**`toBeNull`** : 有些方法在傳入的資料格式不對或欄位缺少時會回傳null，或者某些情況下欄位應該是null值，可以使用。

```js
expect(null).toBeNull()  // passed
```

#### 1-7 使用正規表達式驗證執行結果是否符合預期

**`toMatch`** : 該 API 可以在裏頭帶入正規表達式來驗證結果。

```js
expect('0912345678').toMatch(/^09[0-9]{8}$/)  // passed
```

## 🔗2. 非純值型別比對 (Primitive Types) :

敘述帶補充

#### 2-1 比對值是否完全相等

**`toEqual`** :物件、陣列皆可使用，比對結構是否相同，而非記憶體位置。

```js
expect(['James', 'Curry']).toEqual(['James', 'Curry'])  // passed
expect({ NBA: 'James' }).toEqual({ NBA: 'James' })      // passed

expect({ NBA: 'James' }).toEqual({ NBA: 'Curry' })      // failed  
```

若 toEqual 用來比對物件，物件屬性值為 **`undefined`** 時，會自動忽略該屬性不比對，但若是該值為 **`null`** 時，則會拋出測試失敗。

```js
// passed
expect({ NBA: 'James', team: undefined }).toEqual({ NBA: 'James' }) 

// failed
expect({ NBA: 'James', team: null }).toEqual({ NBA: 'James' }) 
```

**`toStrictEqual`** : 物件、陣列皆可使用。與 toEqual 差別在於物件比對時不會忽略 **`undefined`** 屬性，屬於嚴格比對。

```js
//  passed
expect({ NBA: ['James', 'Curry'], team: undefined }).toEqual({ NBA: ['James', 'Curry'] })

// failed
expect({ NBA: ['James', 'Curry'], team: undefined }).toStrictEqual({ NBA: ['James', 'Curry'] })
```

如果由 Class 物件所產生的實體與物件實字即便結構相等，仍會視為不同。

```js
class NBA {
  constructor(name) {
    this.name = name
  }
}

//  passed
expect(new NBA('James')).toEqual({ name: 'James'})

//  failed
expect(new NBA('James')).toStrictEqual({ name: 'James'})
```

#### 2-2 檢查陣列中是否含有某個值

**`toContain`** : 僅陣列可用。檢查陣列中是否含有某個值。

```js
expect(['James', 'Curry']).toContain('James')  // passed
```

**`toContainEqual`** : 僅陣列可用。若預期結果為純值，則自動檢查該陣列是否含有該值。

```js
expect(['James', 'Curry']).toContainEqual('James')  // passed
expect([{ NBA: 'James' }, { NBA: 'Curry' }]).toContainEqual({ NBA: 'James' })  // passed

expect(['James', 'Curry']).toContainEqual('Kobe')  // failed
```

#### 2-3 檢查資料長度

**`toHaveLength`** : 主要用於陣列。在一些資料長度檢查上非常實用，若用於物件比對則是比對物件中的 **`Length 值`** 。 

```js
expect('NBA球員James').toHaveLength(10)      // passed
expect(['James', 'Curry']).toHaveLength(2)  // passed
expect({ length: 6}).toHaveLength(6)        // passed
```

#### 2-4 檢驗物件是否含有特定屬性

**`toHaveProperty`** : 主要用於物件。檢驗物件是否含有特定 **`屬性`** ，甚至檢查該屬性 **`值`** 是否存在，常用於資料狀態管理來斷言部分資料是否正確。

```js
const Laker = { name: 'James' };

expect(Laker).toHaveProperty('name')           //  passed
expect(Laker).toHaveProperty('name', 'James')  //  passed
expect(Laker).toHaveProperty('name', 'Curry')  //  failed
```

#### 2-5 比對部分物件內容

**`toMatchObject`** : 主要用於物件。類似於 toEqual 中結構比對，差別是 toMatchObject 是比對部分物件中的資料。雖可用來比對陣列資料，但效果比 toContain、toContainEqual 來得差。

```js
const NBA = { Laker: { name: 'James' }, name: 'Curry'}

// passed
expect(NBA).toMatchObject({ name: 'Curry' })
expect(NBA).toMatchObject({ Laker: { name: 'James' }})

// failed
expect(NBA).toMatchObject({ name: 'James' })
expect(NBA).toMatchObject({ Laker: { name: 'Curry' }})
```

## 🔗3. 監聽與模擬 :

## 🔗4. Error :

## 🔗5. 快快照 ( Snapshot ) :
