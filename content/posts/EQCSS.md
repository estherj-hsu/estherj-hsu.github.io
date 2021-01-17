---
title: "EQCSS"
date: 2017-03-14 10:06:02
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "EQCSS"
---
## Notes of EQCSS

### esther
前端技術、觀念持續進步中，快要跟不上惹啊啊啊！！！！！ >O<
今年初發現的EQCSS被我閒置了兩個月後再次開封，細細品嚐這新玩意究竟改變了些什麼，忽然發現這東西不容小覷啊！
但我個人對於「支援各種瀏覽器的版本」這件事情本身就抱持保留態度，所以EQCSS未來是否真能應用到實際專案呢...？
Anyway, 簡單翻譯了一些EQCSS的重點跟背景和相關應用，留檔備用。

<!--more-->

## EQCSS起源 與 為何會需要屬於自己的polyfill

 > [How We Built EQCSS & Why You Should Try Building Your Own Polyfills Too](https://www.sitepoint.com/how-we-built-eqcss-why-you-should-try-building-your-own-polyfills-too/)

<h3 class="esther">esther</h3>

**polyfill** 一詞來自於一種補漆劑品牌，基本上就是用於補足各個瀏覽器與版本之間的差異，達成所有瀏覽器上都能有相同結果的plugin。
簡單來說polyfill可以解決像是舊版瀏覽器不支援問題HTML5新標籤（如section）之類的問題（也包含JS的跨瀏覽器與瀏覽器版本支援問題），當然，主要是以JS去處理，可以參考[html5shiv](https://github.com/aFarkas/html5shiv)。

### Tom Hodgins's Needs

 - 根據元件目前寬/高度指定不同樣式
 - 元件持續保持水平/垂直置中
 - 根據元件內文字長度指定不同樣式
 - 根據元件內子元件數量指定不同樣式
 - 以 `<` 逆向控制父層元件

### Maxime's Answers

 - 依照元件尺寸/文字內容控制樣式
 - 擴展CSS選擇器至父層
 - 擴展CSS正常屬性讓所有元件保持垂直置中

### 5 Challenges of Building EQCSS

#### Syntax Challenges

 - 單純使用CSS
 - 具靈活度，增加開發便利性
 - Future-proof（具未來性的，可與其他程式碼共存）

#### Plugin Challenges

 - 確保檔案大小
 - 原始碼易讀性
 - 結構化插件，獨立IE8

#### Browser Challenges

 - 不同瀏覽器與版本支援
 - 修正Firefox問題

#### Module Challenges

 - 插件模式
 - Webpack/Browserify

#### Documentation Challenges

 - 詞彙
 - 溝通
 - 文件與規範

<h3 class="esther">esther</h3>

作者提及從JS生手到後期可以獨立維護、運作、應用此掛件，以及遇到問題時如何將問題拋向對的人並獲得幫助的三贏策略，當問題被拋出並獲得解決後，受益的往往不僅僅是提問者還包含使用者與解答者，大家都各自在彼此的專業中得到成長。（某方面我覺得這是網路力量造成的巨大優勢啊啊啊！）

## EQCSS

 > [EQCSS: A CSS Extension for Element Queries & More](http://elementqueries.com/)
 > [EQCSS](https://github.com/eqcss/eqcss)
 > [EQCSS spec](https://github.com/tomhodgins/element-queries-spec)

### Element Queries

Element Queries（元件查詢？）是一種RWD的新觀念，響應條件是依據元件本身而非瀏覽器。不同於 `@media` ，使用 `@element` 除了能夠支援瀏覽器尺寸查詢，也能夠針對元件內容與子元件做RWD控制條件。Element Queries重新定義了CSS的範圍概念，帶入了JS的範圍與變數觀念。

### How to use

於頁面中導入JS即可。

```html
<script src=EQCSS.js></script>
```

CSS部分以 EQCSS 格式為主：

```html
<script type="text/eqcss" src=styles.eqcss></script>
```

或者採用 inline 方式：

```html
<script type="text/eqcss">
  ...
</script>
```

#### EQCSS基本格式

```css
@element {selector} and {condition} [ and {condition} ]* { {css} }
```

 - `{selector}` 選擇器，元件本身 Ex: `"#id"` or `".class"`
 - `{condition}` 條件
 - `{css}` 樣式 Ex: `#id div { color: red }`
