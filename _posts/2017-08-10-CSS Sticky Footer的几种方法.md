---
layout:     post
title:      CSS Sticky Footer的几种方法
subtitle:   CSS
date:       2017-08-10
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: CSS
---

## 前言
Sticky Footer的目的：当文档内容少的时候，让它“固定”在浏览器窗口的底部。如果有足够的内容就将页面撑开，footer被撑到文档的底部。
![Sticky Footer](http://s0.qhimg.com/static/2420cb2a1c837ca9.svg)
## 在 content 上加上负的 margin-bottom
HTML:
```
<div class="content">
  <h1>在 content 上加上负的 margin-bottom</h1>
  .....
  <div class="push"></div>
</div>
<div class="footer">Sticky Footer</div>
```
CSS:
```
.content {
  min-height: 100%;
  /* 负的footer高度 */
  margin-bottom: -60px;
}
.footer,
.push {
  /* 相同的高度 */
  height: 60px;
}
```
需要在内容区域加一个额外的元素（.push 元素），这样确保不会因为负 margin 将 footer 提升上来而覆盖了任何实际内容

<iframe height='300' scrolling='no' title='CSS Sticky Footer-1' src='//codepen.io/ywg228/embed/LjLNwy/?height=300&theme-id=0&default-tab=html,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/LjLNwy/'>CSS Sticky Footer-1</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 在 footer 上加上负的 margin-top
HTML:
```
<div class="content">
  <div class="content-inside">
     <h1>在 footer 上加上负的 margin-top</h1>
     .....
  </div>
</div>

<div class="footer">Sticky Footer</div>
```
CSS:
```
.content {
  min-height: 100%;
}
.content-inside {
  /* footer高度 */
   padding-bottom: 60px;
}
.footer {
  height: 60px;
  /* 负的footer高度 */
  margin-top: -60px; 
}
```
跟上面那种方法一样，都有缺点，都添加额外的 HTML 元素。
<iframe height='295' scrolling='no' title='CSS Sticky Footer-2' src='//codepen.io/ywg228/embed/GvEqea/?height=295&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/GvEqea/'>CSS Sticky Footer-2</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 通过 calc 减少 content 高度
HTML:
```
<div class="content">
  <h1>通过 calc( ) 减少 content 高度</h1>
  .....
</div>

<div class="footer">Sticky Footer</div>
```
CSS:
```
.content {
  min-height: calc(100vh - 60px);
}
.footer {
  height: 60px;
}

```

<iframe height='294' scrolling='no' title='CSS Sticky Footer-3' src='//codepen.io/ywg228/embed/qXjadJ/?height=294&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/qXjadJ/'>CSS Sticky Footer-3</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## 使用 flexbox
HTML:
```
<div class="content">
  <h1>使用 flexbox</h1>
  .....
</div>

<div class="footer">Sticky Footer</div>
```
CSS:
```
html {
  height: 100%;
}
body {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
.content {
  flex: 1;
}
```
这种方式堪称完美，就是兼容性有点问题。
