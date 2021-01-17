---
title: "CSS優化項目之一：多語系選單"
date: 2017-09-18 14:11:21
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "CSS opt menu"
---
### esther
想記錄一下自己去年優化的幾個部分，不堪回想的血淚... T^T
剛接觸 sass 時，會被他很厲害的功能迷惑，
曾經我也不自覺地濫用了很多的 `@extend`，
但是如果弄清楚需求與效能後，
會知道很多東西其實維持舊寫法才是比較適當的選擇。
就讓我們繼續看下去吧...

## Needs?

選單的需求很單純：
1. 多階層選單
2. 大項目分類與旗下所有子項目清單
3. 不同語系會有不同寬度與欄數
4. 每一列下方會有分隔線

原本（不是我的）做法是各別將語系的寬度與欄數做了兩個 map 以及一個 map-get 的 function，
先以語系的寬度 map 跑 `@each` 後，再針對語系去 get 該語系欄數的 map，
最後還針對分隔線多了無謂的計算。

寫到這裡應該大概能理解為什麼要優化這塊的CSS了？

<!--more-->

### After optimization?

```sass

// 懶人用專用 font & position & size mixin
=fs($fz: null, $c: null, $lh: null, $fw: null, $fm: null)
  font-size: $fz
  color: $c
  line-height: $lh
  font-weight: $fw
  font-family: $fm
=pos($p: null, $t: null, $r: null, $b: null, $l: null)
  position: $p
  top: $t
  right: $r
  bottom: $b
  left: $l
=size($w: null, $h: null)
  width: $w
  height: $h

// 需要的 map 與 變數
// 不同語系時清單的寬度與每列欄數
$menuListCol: (en-us: (164px, 4), zh-tw: (110px, 6), ko-kr: (142px, 5))
// gutter or gap 看你喜歡怎樣叫
$menuListGap: 30px

// 清單用 flex 唷
.menuList
  align-content: stretch
  box-sizing: content-box
  display: flex
  flex-flow: row wrap
  justify-content: flex-start
  padding: 0 25px

// 分類標題
.menuList-title
  +fs($fz: 18px, $c: #455A64, $fw: normal, $lh: 18px)
  padding-bottom: 10px

// 分類群組
.menuList-group
  align-items: stretch
  margin-right: $menuListGap
  padding: 38px 0 30px 0
  position: relative

// 分類子項
.menuList-item
  +fs($fz: 14px, $c: #999, $lh: 1.4)
  margin: 10px 0 0 0
  &:first-of-type
    margin-top: 0
  &:hover
    color: #455A64

// 重點在這裡，套入 map 內的資料與 gap，針對不同語系時給予不同的寬度
@each $lang, $cols in $menuListCol
  $colW: nth($cols, 1)
  $colN: nth($cols, 2)
  $allW: $colW * $colN + $menuListGap * ($colN - 1)
  html:lang(#{$lang})
    .menuList
      width: $allW
    .menuList-group
      width: $colW
      &:nth-of-type(#{$colN}n)
        margin-right: 0
      &:nth-of-type(#{$colN}n-0)::after
        +pos($p: absolute, $r: 0, $b: 0)
        +size($w: $allW, $h: 1px)
        background-color: #e5e5e5
        content: ''
        display: inline-block
```



> 懶人專用那部分其實就是活用 sass 的 mixin 節省寫 code 時間的好例子，
> 比起 editor 本身的 autocomplete 或是 emmet 的 abbreviations，
> 我反而比較習慣這種用法？（單純討厭背東西）


### Result

<p data-height="400" data-theme-id="dark" data-slug-hash="wrKOKa" data-default-tab="result" data-user="estherj-hsu" data-embed-version="2" data-pen-title="wrKOKa" class="codepen">See the Pen <a href="https://codepen.io/estherj-hsu/pen/wrKOKa/">wrKOKa</a> by Esther (<a href="https://codepen.io/estherj-hsu">@estherj-hsu</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
