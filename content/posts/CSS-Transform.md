---
title: "CSS Transform"
date: 2016-06-01 21:49:35
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "CSS Transform"
---

## A Brief Note of CSS Transform

### esther
Noted by myself in the end of 2015.


<!--more-->

## transform-function

 - rotate 旋轉
 - skew(X,Y) 傾斜
 - scale(X,Y) 變形
 - translate(X,Y) 位移
 - matrix 進階變形矩陣

### rotate(θ)
以參考點為中心軸 2D 旋轉 θ 度。

```sass
transform: rotate(30deg)
```

### skewX(θ)
以參考點為中心軸沿著橫向傾斜 θ 度。

```sass
transform: skewX(20deg)
```

### skewY(θ)
以參考點為中心軸沿著縱向傾斜 θ 度。

```sass
transform: skewY(45deg)
```

### skew(θx,θy)
以參考點為中心軸沿著橫向傾斜 θx 度、 縱向傾斜 θy 度。

```sass
transform: skew(20deg,45deg)
```

> 參數如果只指定 1 個，省略的第 2 個參數，會視為 0 ，也就是只有沿橫向傾斜。

### scaleX(m)
指定元素由參考點橫向縮放 m 倍。

```sass
transform: scaleX(0.5)
```

### scaleY(m)
指定元素由參考點縱向縮放 m 倍。

```sass
transform: scaleY(1.8)
```

### scale(mx,my)
由參考點 2D 橫向縮放 mx 倍、縱向縮放 my 倍，等於是結合 scaleX(mx), scaleY(my) 。

```sass
transform: scale(0.5,1.8)
```

> 參數如果只指定 1 個，省略的第 2 個參數，會等於第 1 個，也就是橫向、縱向以相同比例縮放。

### translateX(o)
由參考點橫向移動 o 距離。

```sass
transform: translateX(60px)
```

### translateY(o)
由參考點縱向移動 o 距離。

```sass
transform: translateY(20%)
```

### translate(ox,oy)
由參考點 2D 橫向移動 ox 距離、縱向移動 oy 距離，結合 translateX(ox), translateY(oy) 。

```sass
transform: translate(60px,20%)
```

> 參數如果只指定 1 個，省略的第 2 個參數，會視為 0 ，也就是只有橫向移動。

亦可同時指定translate、rotate、scale

```sass
transform: translate(180px) rotate(-5deg) scale(0.8)
```

### matrix(a,b,c,d,e,f)

![matrix](https://dev.opera.com/articles/understanding-the-css-transforms-matrix/5.png)

由參考點依據數學變形矩陣 (transformation matrix) 的 6 個參數值產生 2D 變形。(很恐怖。)

```sass
transform: matrix(0,1.611,1.611,0.278,5,5)
```

> Reference
>  - [Understanding the CSS Transforms Matrix](https://dev.opera.com/articles/understanding-the-css-transforms-matrix/)
>  - [Matrix Construction Set](http://www.useragentman.com/matrix/)

## transform-origin

![transform-origin](http://www.108js.com/article/article8/img/css-transforms-matrix2.png)

參考點定義，預設為中心點。

```sass
transform-origin: 0 0 | top left  左上角
transform-origin: 100% 100% | bottom right 右下角
transform-origin: 10px 20px 固定點
```

## frames
影格定義

```sass
// Start to end
@keyframes [animation name]
  from
opacity: 0
  to
opacity: 1

//by frame(%)
@keyframes [animation name]
  0%
transform: translateX(100%)
  30%
transform: translateX(50%)
  100%
transform: translateX(0%)
```

使用方式

```sass
.animation
  display: block
  animation-name: [animation name]
  transform-origin: 0 0
```

## perspective
以透視角度控制3D效果

```sass
perspective: 150px 視角
perspectove: bottom left 基準點控制
```

> perspectiv與perspective-origin是針對子元素做控制，非元素本身，在父層下perspective後針對子層做transform設定即可

```sass
//parent
.wrap
  perspective: 150px

//child
.item
  transform: rotateX(30deg) scale(1.5)
```
