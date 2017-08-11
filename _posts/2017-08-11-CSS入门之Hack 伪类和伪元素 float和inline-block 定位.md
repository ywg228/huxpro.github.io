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
#### 两者区别
- 文档流：浮动元素会脱离文档流，并使得周围元素环绕这个元素。而inline-block元素仍在文档流内。因此设置inline-block不需要清除浮动。当然，周围元素不会环绕这个元素，你也不可能通过清除inline-block就让一个元素跑到下面去。
- 水平位置：很明显你不能通过给父元素设置text-align:center让浮动元素居中。事实上定位类属性设置到父元素上，均不会影响父元素内浮动的元素。但是父元素内元素如果设置了display：inline-block，则对父元素设置一些定位属性会影响到子元素。（这还是因为浮动元素脱离文档流的关系）。
- 垂直对齐：inline-block元素沿着默认的基线对齐。浮动元素紧贴顶部。你可以通过vertical属性设置这个默认基线，但对浮动元素这种方法就不行了。这也是我倾向于inline-block的主要原因。
- 空白：inline-block包含html空白节点。如果你的html中一系列元素每个元素之间都换行了，当你对这些元素设置inline-block时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴。

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
