---
layout:     post
title:      CSS入门之Hack 伪类和伪元素 float和inline-block 定位
subtitle:   CSS
date:       2017-08-11
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: CSS
---

## CSS hack
不同浏览器对CSS的兼容不同，需要hack来对CSS做一些兼容。有四种方法：
- 条件注释法
这种方式是IE浏览器专有的Hack方式，微软官方推荐使用的hack方式。
```
<!--[if lt IE 9]>
  <!--html5shiv解决HTML5新元素不被IE6-8识别的问题-->  
  <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
<![endif]-->
```
- 类内属性前缀法
在CSS样式属性名前加上一些只有特定浏览器才能识别的hack前缀，以达到预期的页面展现效果。
```	
background-color:blue !important; /* All browsers but IE6 */
```
- 选择器前缀法
- CSS3选择器结合JavaScript的Hack

## float和inline-block
#### 作用
相邻元素会在一行显示不换行，直到本行排满，就换行显示。

#### 两者区别
- 文档流：float会脱离文档流，导致重绘，增加浏览器消耗。inline-block不脱离文档流。
- 水平位置：给float的父元素设text-align: center，不会让float居中。给inline-block的父元素设text-align: center，会让inline-block居中。
- 垂直对齐：浮动元素默认顶部对齐vertical-align: top，如遇到上行有空白，而当前元素大小可以挤进去，那么这个元素会在上行补位排列。而inline-block元素默认基线对齐vertical-align: baseline。当一行内的元素高度不一时，以高度最大的元素高度为行高，即使高度小的元素周围有空，也不会有第二行元素上浮补位。由于元素的容器属性为block，内容为inline，所以可以视为文字，然后通过vertical设置垂直对齐方式。
- 空白：inline-block元素之间如果包含html空白节点，元素之间会出现空白间隙。而浮动元素会忽略空白节点，互相紧贴。

#### 浮动导致父元素高度塌陷
解决：清除浮动 在父元素加上clearfix
```
.clearfix:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  font-size: 0;
  visibility: hidden;
}
.clearfix {
  *zoom: 1; /* For IE 6&7 only 可省略*/
}
```

#### float元素换行后会自动填充到前一行比较矮的元素下面
给各个float元素设置固定高度。

#### 去除inline-block元素的空白间距
![inline-block元素的空白间距](https://cloud.githubusercontent.com/assets/12554487/21287532/db3c7752-c4a8-11e6-81df-81ce704d7955.png)

给父元素设置font-size: 0，子元素需重新设置字体大小。
```
<div class="parent">
  <div class="child">inline-block</div>
  <div class="child">inline-block</div>
  <div class="child">inline-block</div>
</div>
```
```
.parent {
    font-size: 0;
}
.child {
    display: inline-block;
    font-size: 14px;
}
```
#### 使用场景
- 当若干子元素居左，若干子元素居右，使用float。
- 要设置某些子元素在一行或者多行内显示，尤其是排列方向一致的情况下，尽量使用inline-block。
- 若干个元素平行排列，且在父元素中居中排列，此时可以用inline-block，且给父元素设text-align: center。
- 用inline-block可以实现横向导航栏，无论是居左的导航栏还是居右的都适用，尽管用float也可以实现，但是推荐用inline-block实现。

## 伪类和伪元素
#### 定义
- **伪类用于当已有元素处于的某个状态时，为其添加对应的样式** <br>
这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过:hover来描述这个元素的状态。虽然它和普通的css类相似，可以为已有的元素添加样式，但是它只有处于dom树无法描述的状态下才能为元素添加样式，所以将其称为伪类。<br>

- **伪元素用于创建一些不在文档树中的元素，并为其添加样式** <br>
比如说，我们可以通过:before来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。

#### 例子
HTML:
```
<ul>
    <li>item1</li>
    <li>item2</li>
    <li>item3</li>
</ul>
```
CSS:
```
        ul {
            width: 200px;
            list-style-type: none;
        }

        li {
            width: 100%;
            height: 30px;
            background-color: #fff;
            text-align: center;
        }
        /*在被选元素前插入内容。需要使用content属性来指定要插入的内容。被插入的内容实际上不在文档树中。*/
        ul:before { 
            width: 200px;
            height: 40px;
            line-height: 40px;
            text-align: center;
            background-color: orange;
            display: inline-block;
            content: 'header';
        }

        li:nth-child(even) {
            background-color: #ccc;
        }
```

<iframe height='271' scrolling='no' title='CSS　伪元素' src='//codepen.io/ywg228/embed/aywjBp/?height=271&theme-id=0&default-tab=css,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen <a href='https://codepen.io/ywg228/pen/aywjBp/'>CSS　伪元素</a> by Mr.Yang (<a href='https://codepen.io/ywg228'>@ywg228</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

#### 区别
伪类操作的是文档树中已有的元素，而伪元素则创建了一个文档树外的元素。 <br>
所以伪类与伪元素的区别在于：**有没有创建一个文档树之外的元素**。

#### 图片
![伪类](http://www.alloyteam.com/wp-content/uploads/2016/05/%E4%BC%AA%E7%B1%BB.png)
![伪元素](http://www.alloyteam.com/wp-content/uploads/2016/05/%E4%BC%AA%E5%85%83%E7%B4%A0.png)

#### 伪元素使用单冒号还是双冒号？
CSS3规范中的要求使用双冒号(::)表示伪元素，以此来区分伪元素和伪类，比如::before和::after等伪元素使用双冒号(::)，:hover和:active等伪类使用单冒号(:)。除了一些低于IE8版本的浏览器外，大部分浏览器都支持伪元素的双冒号(::)表示方法。
