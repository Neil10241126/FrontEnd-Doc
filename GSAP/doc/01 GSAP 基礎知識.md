# 01 GSAP 基礎知識
---

## 建立動畫
語法由 `方法`、`標籤元素`、`控制項` 所組成，這裡有個 [CodePen](https://codepen.io/GreenSock/pen/RwQYNNx) 範本。
```js
gsap.to('.box', { x: 200 })
```

- 方法 :
  ```js
  gsap.to(selector, vars) // 從原始值轉換為自訂值。

  gsap.from(selector, vars) // 自定義起始值、結束值。

  gsap.fromTo(selector, varsA, varsB) // 自定義起始值、結束值。
  ```
- 標籤 : 可以是標籤選擇器、class選擇器、ID選擇器、變數。
- 控制項 : 由物件組成 { } ，包裹簡化的 CSS 語法 ，控制動畫的生成方向。

## CSS 及 GSAP 語法差異

以往 CSS 寫 transform

```css
transform: rotate(360deg) translateX(10px) translateY(50%);
```

GSAP 寫 transform

```js
{ rotate: 360, x: 10, yPercent: 50 }
```
## GSAP、CSS 語法比較表
| GSAP | CSS | 定義 |
| :---         |     :---:      | :--- |
| x : 100 | transform : translateX ( 100 px ) | 水平移動 100 px |
| y : 100 | transform : translateY ( 100 px ) | 垂直移動 100 px |
| xPercent : -50 | transform : translateX ( - 50% ) | 水平移動 50% |
| yPercent : -50 | transform : translateY ( - 50% ) | 垂直移動 50% |
| rotation : 360 | transform : rotate ( 360 deg ) | 旋轉 360 度 |
| scale : 2 | transform : scale ( 2, 2 ) | 放大或縮小 |
| transformOrigin : " 0% 100% ” | transform-origin : 0% 100%  | 平移的中心點 ( 這會以左下角旋轉 ) |


<details>
<summary>GSAP 內建屬性</summary>

```js
{
  duration: 0.5,    // 動畫持續幾秒 ( 預設值 : 0.5 )
  delay: 2,         // 動畫開始前延遲 ( 秒 )
  repeat: 1,        // 動畫重複次數 ( -1 為無限重複 )
  yoyo: fasle,      // 如果為 true ，將重複反向運行
  stagger: 0.5,     // 每個動畫開始的間格時間 ( 秒 )
  ease: power1.out, // 控制動畫的變化率 ( 預設值 : power1.out )
}
```
</details>
