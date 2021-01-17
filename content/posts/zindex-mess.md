---
title: "z-index 之亂"
date: 2016-12-09 10:22:23
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "zindex mess"
---

### esther
再次強調：*所有強人分享的方法都有優缺點，也都有適用性的問題*。  
找到好方法，要先能完整理解概念後，再來思考如何針對自己需求做調整，不要只知道照抄，若不了解其中原理，用到最後也會失去方向。  
另外，對頁面圖層概念若不清楚，有再好的工具/方法都無法做到正確管理 `z-index`！  
盡量藉由切版來避免元件中子元件的層次問題，而非一昧的堆疊元件後再利用工具/方法去做 `z-index` 管理，這種方式只會本末倒置！

在前陣子的地獄 CSS 重構期間，一直很困惑明明使用了 z-index map 做管理（源自 Hugo Giraudel），卻還是遇到其他開法者無法有效率且正確使用 `z-index` 問題。

搜尋 `z-index` 管理相關文章時，剛好看到 [MUKI](http://muki.tw/tech/sass-z-index-management/) 針對 sass 控管 `z-index` 圖層的文章，才發現原來 [Hugo Giraudel](http://www.sassmeister.com/gist/11172138) 的概念是來自於 Jackie Balzer 的 [Sassy Z-Index Management For Complex Layouts](https://www.smashingmagazine.com/2014/06/sassy-z-index-management-for-complex-layouts/)。

花了點時間研究了這篇文章的概念，基本上比較符合目前公司超大型專案的架構，索性整理了一下兩者的差異和優缺點：
 - Balzer 的解法是針對（超）大型專案下，大架構本身需要圖層管理，內部小專案的元件也需要圖層管理的情境，可以有類似樹狀圖概念的管理方式。
 - Giraudel 的解法則是單純針對一般專案下的單一層次管理，適用架構本身不複雜的專案。


<!--more-->


### Why z-index?

Jackie Balzer 的文章中有提到三點使用 `z-index` 時須釐清的問題：

 1. 為何此元件需要使用 `z-index`？這個元件代表的意義是？
 1. 適合此元件的位置次序是什麼？若增加是否會影響其他元件？
 1. 若增加此元件的 `z-index`，哪些其他的元件次序同時也會被增加？

以我遇到的情況而言，其他開發者針對單一專案就增加了6層的圖層，但實際上都是小元件（如按鈕），這樣的狀況除了切版本身（不當堆疊）的問題外，我也發現若對圖層概念不清楚，即便有再好的管理工具，問題也會持續存在（然後隨著專案越大，就越亂惹 😱）。

因此，若能在使用 `z-index` 之前釐清這三點，勢必會對 `z-index` 的使用有更完整的概念，也能更好管理。


### Jackie Balzer 的 z-index 管理方式

elements 為外層架構元件，modal-elements 為外層架構中 modals 的子元件，每增加一個元件就會自動排序，這種做法可以避免不同大型元件中的 `z-index` 交互影響，也可以避免元件內的子元件層次過多時排序混亂的問題。

```sass
// 變數清單
$elements: project-covers, user-tooltip, sorting-bar, modals
// 變數清單：modals 子元件
$modal-elements: fields, form-controls, errors, autocomplete-dropdown
// z-index 呼叫
.project-covers
  z-index: index($elements, project-covers)                   // z-index: 1
.user-tooltip
  z-index: index($elements, user-tooltip)                     // z-index: 2
.sorting-bar
  z-index: index($elements, sorting-bar)                      // z-index: 3
.modal
   z-index: index($elements, modals)                          // z-index: 4
   .field
      z-index: index($modal-elements, fields)                 // z-index: 1
   .form-controls
      z-index: index($modal-elements, form-controls)          // z-index: 2
   .error
      z-index: index($modal-elements, errors)                 // z-index: 3
   .autocomplete-dropdown
      z-index: index($modal-elements, autocomplete-dropdown)  // z-index: 4
```


### Hugo Giraudel 的 z-index 管理方式

這種方式是單純使用 sass 的 maps 做管理，可以針對不同元件手動設定需要的 `z-index`，缺點是單一 map 管理的情況下，無法解決多框架下的多層次問題，且必須依靠手動調整所有元件的 `z-index` 值來做設定，若在共同開發的情況下，開發者還是須要了解每個元件的層次結構，才能避免交互影響，另一個需注意的問題是，`z-index` 為手動輸入，某方面不太符合 `z-index` 簡要使用方式。

```sass
// z-index maps
$z-layers: (
  'modal':              5000,
  'dropdown':           4000,
  'default':               1,
  'bottomless-pit':   -10000
)

// function for z-index map-get
@function z($layer)
  @if not map-has-key($z-layers, $layer)
    @warn "No z-index found in $z-layers map for `#{$layer}`. Property omitted."
  @return map-get($z-layers, $layer)

// z-index calling
.modal
  z-index: z("modal")                        // z-index: 5000
.modal-backdrop
  z-index: z("modal") + 1                    // z-index: 5001
.dropdown
  z-index: z("dropdown")                     // z-index: 4000
.element:after
  z-index: z("default") - 2                  // z-index: -1
.im-falling
  z-index: z("bottomless-pit")               // z-index: -10000
.error
  z-index: z("unknown-z-index")              // z-index: ?
```


----

以下是我針對 Giraudel 的架構做多元件管理提供的簡單範例（省略 function 部分）

```sass
// z-index maps
$z-layers: (
  'header':               100,
  'goTop':                200,
  'popupBox':             300
)

// z-index calling
.header
  z-index: z("header")                        // z-index: 100
  .header-logo
    z-index: z("header") + 1                  // z-index: 101
.goTop
  z-index: z("goTop")                         // z-index: 200
.popupBox
  z-index: z("popupBox")                      // z-index: 300
  .popupBox-mask
    z-index: z("popupBox") - 1                // z-index: 299
  .popupBox-msg
    z-index: z("popupBox") + 1                // z-index: 301
```
