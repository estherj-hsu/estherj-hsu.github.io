---
title: "CSS Pixel Art"
date: 2017-09-01 13:40:32
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "CSS Pixel Art"
---
## Pixel Art

看了 Una Kravets 的純 CSS 遊戲，覺得 Pixel 實在太有趣，就自己跟著做了一個。
基本概念是用迴圈去跑出map 中的 x 軸和 y 軸 ，然後套用在 `box-shadow` ，完全活用了 sass 的幾個特性：

 - function
 - map
 - for

> Ref: [CSSconf EU 2017 | Una Kravets: Let’s Build a CSS Game](https://youtu.be/WmVH85G59Lk)

<iframe width="560" height="315" src="https://www.youtube.com/embed/WmVH85G59Lk" frameborder="0" allowfullscreen></iframe>

<!--more-->

### How?

首先，設定 px 的尺寸，並建立需要顏色與 pixel 的 map，基本上可以從 map 看出 pixel 的內容。

```css
$pxSize: 10px

$colors: (
  'o': transparent,
  'b': #000
);

$pixel-art: (
  esther: (
    (b b b b b b b b b b b b b b b b b b b b b b b b b)
    (b o o o b b o o b o o o b o b o b o o o b o o b b)
    (b o b b b o b b b b o b b o b o b o b b b o b o b)
    (b o o o b b o b b b o b b o o o b o o o b o o b b)
    (b o b b b b b o b b o b b o b o b o b b b o b o b)
    (b o o o b o o b b b o b b o b o b o o o b o b o b)
    (b b b b b b b b b b b b b b b b b b b b b b b b b)
  )
)
```

接著以 function 搭配 for 跑 x 與 y 軸

```css
@function pixelize($matrix, $pxSize){
  $sh: '';
  $rows: length($matrix);

  @for $row from 1 through $rows {
    $row-num: nth($matrix, $row);

    @for $col from 1 through length($row-num) {
      $dot: nth($row-num, $col);

      $sh: $sh + ($pxSize*$col) + ' ' + ($pxSize*$row) + ' ' + map-get($colors, $dot);

      @if not ($col == length($row-num) and $row == $rows) {
        $sh: $sh + ',';
      }
    }
  }
  @retur unquote($sh);
}
```

接著我將原本的 `$pxSize` 做了一些調整，讓各個 pixel 可以擁有自己的 px 尺寸，並將完整的 px 塞入 `::before` 中。

```css
$pixel-list: (esther: 5px);

@each $item, $pxSize in $pixel-list {
  .pixel-#{$item} {
    height: $pxSize*length(map-get($pixel-art, $item));
    width: $pxSize*length(nth(map-get($pixel-art, $item), 1));
    &::after {
      top: -$pxSize;
      left: -$pxSize;
      height: $pxSize;
      width: $pxSize;
      content: '';
      position: absolute;
      box-shadow: pixelize(map-get($pixel-art, $item), $pxSize);
    }
  }
}

[class*='pixel'] {
  position: relative;
  display: inline-block;
  vertical-align: middle;
}

```

整體的概念滿簡單的，玩起來也滿治癒的，只是要拿這做完整的遊戲還真的得好好想想了！

### Result

<iframe height='265' scrolling='no' title='pixel-art' src='//codepen.io/estherj-hsu/embed/yMvBEa/?height=265&theme-id=dark&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/estherj-hsu/pen/yMvBEa/'>pixel-art</a> by Esther (<a href='https://codepen.io/estherj-hsu'>@estherj-hsu</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
