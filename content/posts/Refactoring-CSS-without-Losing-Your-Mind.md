---
title: "Refactoring CSS without Losing Your Mind"
date: 2016-10-17 11:10:15
hidden: false
draft: false
tags: ["CSS/sass"]
slug: "Refactoring CSS"
---
## WEBEXPO | By Harry Roberts

<div id="presentation-embed-38898201"></div>
<script src='http://slideslive.com/embed_presentation.js'></script>
<script>
    embed = new SlidesLiveEmbed('presentation-embed-38898201', {
        presentationId: '38898201',
        autoPlay: false // change to true to autoplay the embedded presentation
    });
</script>

### esther

少數針對CSS重構的討論，喜歡講者一開始針對technical debt的解說和refactoring/rewrite的區分。
劃重點：Use `all: initial;` to buy you more time! XDDDD
另外作者提到的小故事也滿讓人值得思考的：
Refactoring is a second chance that most industries don't get.

### Reference

 - [3 Essential Tips for Refactoring CSS Without Losing Your Mind](https://www.shopify.com/partners/blog/3-essential-tips-for-refactoring-css-without-losing-your-mind-from-harry-roberts)

<!--more-->

## Personal notes
### Three Kinds of Refactoring
1. As-You-Go
2. **Technical Debt**
3. Rewrites and Overhauls

### Technical Debt
We're going to incur some of it, fact.
It's vitally important that we keep up repayments.
People forget that debt repayments incur interest.
Schedule in bug-fixing and tech-debt cleanup every sprint.
Make and prove the business case for refactoring.

### When Not to Refactor
 - If you're not actually being slowed down by something.
 - If it's something that can be ignored or avoided.
 - If it's something that can be captured by a rewrite later on.
 - If a rewrite is the better solution.

### Tips
 - defence.css
   - `all: initial;`
 - .RF-* Classes
   -  `[class*= 'rf-']` = refactoring done
   -  `[class]:not([class*= 'rf-'])` = work left to do
 - hacking specificity (hacks are alway not nice)
   - *Ideally*: Refactor until you don't need the hacks.
   - *Realistically*: Use one of these hacks to solve the problem.
   - *Never*: Use `!important`.
 - shame.css (isolate hacks / self-writing todo list)


### Remember...
 - Prevention is than the cure.
 - Technical Debt is fine, just make sure you keep up repayments.
 - Only refactoring once you can see tangible benefits.
 - Avoid long Refactoring Tunnels.
 - Isolate and highlight both hacks and refactored work.
