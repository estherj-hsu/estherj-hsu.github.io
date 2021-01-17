---
title: "CSS Architecture"
date: 2016-10-26 10:37:46
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "CSS Architecture"
---
## Dayton Web Developer | By Nathan Rambeck

<iframe src="https://player.vimeo.com/video/172444121?portrait=0" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

<br>

### esther
CCS架構到命名方式的應用與舉例，沒有細部介紹現有的架構跟命名規則有點可惜，有大概提到幾個命名方式的優缺點，Naming Methodologies跟Namespacing內容不錯。
ITCSS & SUIT CSS 可以再深入研究。（筆記）

<!--more-->

### Reference
 - [Thoughtful CSS Architecture](https://seesparkbox.com/foundry/thoughtful_css_architecture)
 - [itcss](http://itcss.io/)


## CSS Pattern Libraries
 - Fabricator
 - PatternLab
 - Google Material Design


## CSS Architecture
 - OOCSS (re-usability)
 - SMACSS (seperation of concerns)
 - ITCSS (style ordering)

## Type of CSS Rules
 - *Base*: styles applied globally to bare elements
 - *Objects*: Re-usable CSS classes for layout and structure
 Ex. grid systems / layout containers / structural patterns
 - *Components*: Components are discrete, self-contained pieces of UI
 Ex. buttons / carousels / pullquotes / header / navigation ... etc.
 - *State*: State classes begin with "is" or "has"
 - *Themes*: Theme classes alter components with unique colors, fonts or other decorations
 - *Utilities*: Utility classes are single purpose helpers that apply one specific styling rule

## Naming Your Classes
 - All lowercase
 - Use dashes or underscores
 - Long enough to discern (.pullquote not .pq)
 - No longer than needs to be (.btn not .button)

 > [Get BEM](http://getbem.com/)

### Naming Methodologies
 - By presentation (how they look)
Ex. `button--green`, `rounded-image`
 - By content (what is in)
Ex. `profile-image`, `article-heading`
 - By function (what it for)
Ex. `button--primary`, `content-heading`

 > Reference: [Naming CSS Stuff is Really Hard](https://seesparkbox.com/foundry/naming_css_stuff_is_really_hard)

### Namespacing
Add prefixes to all of your classes to recognize the purpose of each one.
 - Objects: `.o-`
 - Components: `.c-`
 - State: `.is-` or `.has-`
 - Theme: `.t-`
 - Utility: `.u-`

### Advantages of Using PREPROCESSOR
 - Imports
 - Variables
 - Functions
 - Mixins
 - Nesting
