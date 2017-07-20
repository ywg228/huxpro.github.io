---
layout:     post
title:      Sublime Text3的插件安装和使用
subtitle:   编辑器
date:       2017-07-20
author:     Ywg
header-img: img/home-bg-o.jpg
catalog:    true
tags: 工具
---
## 前言
Sublime Text 3是一款强大而精巧的文本编辑器。下载地址：http://www.sublimetext.com/3
- 界面友好、功能非凡、性能极佳
- 可令代码高亮、语法提示、自动完成
- 支持众多插件扩展——锦上添花、强之又强

## Package Control管理插件安装
Package Control是原来管理插件的插件，首次使用时需安装。
- 使用Ctrl + `（Esc键下方）快捷键或者通过 View -> Show Console菜单打开命令行
![](http://images.cnblogs.com/cnblogs_com/hykun/607566/o_2-1.jpg)
- 将下面代码复制后粘贴到如上图中“<代码粘贴处>”，然后回车，或在 https://packagecontrol.io/installation 查看
``` 
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
``` 
- 稍等片刻后，如果安装成功，在Preferences菜单下可以看到Package Settings和Package Control两个菜单。
- 若不能通过以上方式成功安装，点击下载https://sublime.wbond.net/Package%20Control.sublime-package并复制到安装目录下的Installed Packages目录

## 插件安装
- 在Sublime Text 3中按下快捷键Ctrl+Shift+P，在出现的文本框中输入Install Package(或直接输入“ip”)选中Install Package并回车
- 然后输入或选择你需要的插件再按回车就可以安装插件了

## 常用插件
#### Emmet
- Emmet插件可以说是使用Sublime Text进行前端开发必不可少的插件，它让编写HTML代码变得极其简单高效。
- 基本用法：输入标签简写形式，然后按Tab键。 
- 更多用法：[速查表](https://docs.emmet.io/cheat-sheet/)

#### JSFormat
- JS代码格式化插件。
- 使用方法：ctrl + alt + f

#### SublimeCodeIntel
代码提示插件

#### SublimeLinter
代码校验工具

#### HTML-CSS-JS Prettify
- 格式化代码
- 调用方法：Ctrl+Shift+H 或者 右键 -> HTML/CSS/JS Prettify -> Prettify Code

#### SideBarEnhancements
侧栏右键功能增强，非常实用

#### AutoFileName 
- 可以自动补全文件路径及名称的插件 
- 调用方法：`<img src="../" />`

#### CssComb
- 对CSS属性进行排序和格式化，依赖于Node.js 
- 使用方法：菜单Tools->Run CSScomb或在CSS文件中按快捷键Ctrl+Shift+C

#### Autoprefixer
- CSS3私有前缀自动补全，依赖于Node.js <br>
- 使用方法：在输入CSS3属性后（冒号前）按Tab键

#### DocBlockr
- 代码快注释插件
- 调用方法：输入 /** 后按 Enter 或者 Tab

#### BracketHighlighter
- 匹配标签高亮的小插件，可以把匹配到的如 {}、()、”、””等对应的符号或者标签高亮显示。
- 调用方法：选中标签即可显示

#### Browser Refresh
- 可以实现保存文件，切换到浏览器并自动刷新浏览器来查看更改结果。
- 使用方法： 默认为Ctrl+Shift+R，可自行修改

#### IMESupport
解决中文输入框不跟随的问题

#### Alignment
自动对齐代码，包括PHP、CSS、JavaScript语言。使得代码看起来更整齐美观，更具可读性

#### jQuery
- 自动补全 jQuery 函数的插件，带有语法高亮，并且包含几乎所有的 jQuery 方法。
## 常用快捷键
- HTML标签后Tab：自动补全
- Ctrl + `：打开控制台
- Ctrl + 滚动：字体缩放
- 
``` 

``` 
