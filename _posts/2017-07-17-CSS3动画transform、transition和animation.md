---
layout:     post
title:      CSS3动画transform、transition和animation
subtitle:   CSS3学习
date:       2017-07-17
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: CSS3
---

## 前言
CSS3动画包括transform、transition和animation这三个属性。
为了浏览器兼容，需要在属性前加上-webkit-、 -ms- 和 -moz- 前缀。

## transform
允许我们对元素可移动（translate）、旋转（rotate）、缩放（scale）、倾斜（skew）。

#### 2D变换

移动 | 描述
------------ | -------------
translateX(tx)  | 沿X轴方向移动 单位px
translateY(ty) | 沿Y轴方向移动
translate(tx,ty) | 沿X和Y轴方向移动

缩放 | 描述
------------ | -------------
scaleX(sx) | 沿X轴方向缩放即宽度缩放
scaleY(sy) | 沿Y轴方向缩放即高度缩放
scale(sx,sy) | 沿X和Y轴方向缩放即宽度和高度一起缩放, 一个参数则宽高缩放同等倍数

2D旋转 | 描述
------------ | -------------
rotate(angle) | 从原点（由 transform-origin 属性指定）开始顺时针方向旋转一个特定的角度 单位deg 负值则为逆时针旋转

倾斜 | 描述
------------ | -------------
skewX(angle) | 沿X轴方向以指定的角度倾斜 单位deg
skewY(angle) | 沿Y轴方向以指定的角度倾斜
skew(ax,ay) | 沿X轴和Y轴方向以指定的角度倾斜

矩阵 | 描述
------------ | -------------
matrix(a, c, b, d, tx, ty) | 定义 2D 转换，使用六个值的矩阵。matrix() 方法把所有 2D 转换方法组合在一起。matrix() 方法需要六个参数，包含数学函数，允许对元素进行旋转、缩放、移动以及倾斜。

#### 3D变换

3D移动 | 描述
------------ | -------------
translateZ(tz)  | 沿Z轴方向移动，值越大，元素也离观看者越近，视觉上元素就变得更大；反之则元素离观看者越远，视觉上元素就变得更小。
translate3d(tx,ty,tz) | 三者的组合 推荐

3D缩放 | 描述
------------ | -------------
scaleZ(sz)  |  
scale3d(sx,sy,sz) |  scale3d(1,1,sz)等同于scaleZ(sz) scaleZ()和scale3d()函数单独使用时没有任何效果，需要配合其他的变形函数一起使用才会有效果。

3D旋转 | 描述
------------ | -------------
rotateX(angle) | 以X轴，从下向上旋转 rotateX(a)等同于rotate3d(1,0,0,a)
rotateY(angle) | 以y轴，从左向右旋转 rotateY(a)等同于rotate3d(0,1,0,a)
rotateZ(angle) | 以原点，顺时针旋转 rotateZ(a)等同于rotate3d(0,0,1,a)
rotate3d(x,y,z,angle) | 

``` 
x：是一个0到1之间的数值，主要用来描述元素围绕X轴旋转的矢量值；
y：是一个0到1之间的数值，主要用来描述元素围绕Y轴旋转的矢量值；
z：是一个0到1之间的数值，主要用来描述元素围绕Z轴旋转的矢量值；
a：是一个角度值，主要用来指定元素在3D空间旋转的角度，如果其值为正值，元素顺时针旋转，反之元素逆时针旋转。
``` 

3D矩阵 | 描述
------------ | -------------
matrix(a, c, b, d, tx, ty) | 定义 2D 转换，使用六个值的矩阵。matrix() 方法把所有 2D 转换方法组合在一起。matrix() 方法需要六个参数，包含数学函数，允许对元素进行旋转、缩放、移动以及倾斜。


#### 3D Transform属性

属性 | 描述
------------ | -------------
backface-visibility | 指定元素背面对于观察着是否可见 visible可见 hidden不可见(常用) 常见翻牌效果，我们不希望元素内容在背面可见。
perspective | 指定了观察者与z=0平面的距离，使具有三维位置变换的元素产生透视效果。
perspective-origin | 指定了观察者的位置 默认为'50% 50%'(也就是center)
transform-origin | 允许你改变被转换元素的位置。默认变形的原点在元素的中心点，或者是元素X轴和Y轴的50%处，
transform-style | 指定该元素的子元素是（看起来）位于三维空间内，还是在该元素所在的平面内被扁平化。常用preserve-3d 3d模式

**perspective**
``` 
perspective取值为none或不设置，就没有真3D空间。
perspective取值越小，用户与3D空间Z平面距离越近，3D视觉效果就越明显，也就是你的眼睛越靠近真3D。
perspective的值无穷大，或值为0时与取值为none效果一样。
``` 

**transform-origin**
``` 
 transform-origin取值
 top = top center = center top = 50% 0
 right = right center = center right = 100%或(100% 50%)
 bottom = bottom center = center bottom = 50% 100%
 left = left center = center left = 0或(0 50%)
 center = center center = 50%或（50% 50%）
 top left = left top = 0 0
 right top = top right = 100% 0
 bottom right = right bottom = 100% 100%
 bottom left = left bottom = 0 100%
``` 

<iframe height='265' scrolling='no' title='gRyKyx' src='//codepen.io/ywg228/embed/gRyKyx/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/gRyKyx/'>gRyKyx</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>


## transition
允许css属性值在一定的时间区间内平滑地过渡
#### 属性
- transition-property css属性名 或 all
- transition-duration 过渡效果需要的时间，默认为0 单位ms(毫秒)或s(秒)
- transition-timing-function 过渡效果的时间曲线，默认为ease，还有linear、ease-in、ease-out、ease-in-out、cubic-bezier。
- transition-delay 过渡效果延迟时间，默认为0
- 简写形式：transition: property duration timing-function delay;

![transition](http://images0.cnblogs.com/blog/707050/201507/121927450496803.png)

#### 例子

<iframe height='265' scrolling='no' title='MoRGge' src='//codepen.io/ywg228/embed/MoRGge/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/MoRGge/'>MoRGge</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

#### JS检测过渡效果是否完成
当过渡效果完成时触发transitionend事件, 在WebKit下是webkitTransitionEnd
``` 
el.addEventListener("transitionend", function() {
  //执行动画结束后的操作
, false);
``` 

#### 注意点
- transition需要事件触发，没法在网页加载时自动发生
- transition是一次性的，不能重复发生，除非再触发
- transition只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态
- 一条transition规则，只能定义一个属性的变化，不能涉及多个属性

## animation + @keyframes
创建帧动画，元素从一种样式逐渐变化为另一种样式的效果。

#### 属性
- @keyframes 规定动画 后加动画名 "from" 和 "to"等同于 0% 和 100%。
- animation-name  @keyframes 的动画名
- animation-duration 动画完成一周期需要的时间，默认为0 单位s或ms
- animation-timing-function 动画的速度曲线，默认为ease 
- animation-delay 动画延迟时间，默认为0 
- animation-iteration-count 动画播放的次数。默认是 1 循环播放为infinite
- animation-direction 设置动画在每次运行完后是反向运行还是重新回到开始位置重复运行，默认为normal
- animation-play-state 设置画是否正在运行或暂停，默认为running
- animation-fill-mode 指定动画执行前后如何为目标元素应用样式。
- 简写形式：animation: name duration anim;

#### 例子
<iframe height='265' scrolling='no' title='LLvdaO' src='//codepen.io/ywg228/embed/LLvdaO/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/LLvdaO/'>LLvdaO</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

<iframe height='265' scrolling='no' title='MoRPQP' src='//codepen.io/ywg228/embed/MoRPQP/?height=265&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/MoRPQP/'>MoRPQP</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

## CSS3无缝轮播图

<iframe height='300' scrolling='no' title='WEbPMm' src='//codepen.io/ywg228/embed/WEbPMm/?height=265&theme-id=0&default-tab=result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/WEbPMm/'>WEbPMm</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

##  JS捕获CSS3的动画事件
每当动画事件发生时，都会调用AnimationListener函数
```
function PrefixedEvent(element, type, callback) {
    var pfx = ["webkit", "moz", "MS", "o", ""];
    for (var p = 0; p < pfx.length; p++) {
        if (!pfx[p]) type = type.toLowerCase();
        element.addEventListener(pfx[p] + type, callback, false);
    }
}
var animEle = document.getElementById("anim");
PrefixedEvent(animEle, "AnimationStart", AnimationListener);  //动画开始触发
PrefixedEvent(animEle, "AnimationIteration", AnimationListener); //每一次新的动画执行过程中被触发
PrefixedEvent(animEle, "AnimationEnd", AnimationListener); //动画结束触发
```
