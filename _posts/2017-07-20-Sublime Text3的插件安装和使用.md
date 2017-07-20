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
- 自动对齐代码，包括PHP、CSS、JavaScript语言。使得代码看起来更整齐美观，更具可读性
- 使用：先选择要对齐的文本 > 按快捷键Ctrl+Alt+A

#### jQuery
- 自动补全 jQuery 函数的插件，带有语法高亮，并且包含几乎所有的 jQuery 方法。

#### Git
- 插件基本上实现了git的所有功能
- 使用：https://github.com/kemayo/sublime-text-git/wiki

#### Nodejs
- node代码提示
- 教程：https://sublime.wbond.net/packages/Nodejs

####  HTML5
- 支持hmtl5规范的插件包
- 使用方法：新建html文档 -> 输入HTML5 -> 敲击Tab -> 自动补全html5规范文档

#### ColorPicker
- 调色板
- 使用方法：ctr + shift + c

#### convertToUTF8
可以是使一些在 Sublime Text 3 中打开乱码的文件正常显示。

#### LESS
- LESS代码高亮插件
- 打开.less文件或者设置为less格式

## 常用快捷键
#### 选择类
快捷键 | 功能
------------ | -------------
Ctrl+D	| 选中光标所占的文本，继续操作则会选中下一个相同的文本。
Alt+F3	| 选中文本按下快捷键，即可一次性选择全部的相同文本进行同时编辑。举个栗子：快速选中并更改所有相同的变量名、函数名等。
Ctrl+L	| 选中整行，继续操作则继续选择下一行，效果和
Shift+↓	| 效果一样。
Ctrl+Shift+L	| 先选中多行，再按下快捷键，会在每行行尾插入光标，即可同时编辑这些行。
Ctrl+Shift+M	| 选择括号内的内容（继续选择父括号）。举个栗子：快速选中删除函数中的代码，重写函数体代码或重写括号内里的内容。
Ctrl+M	| 光标移动至括号内结束或开始的位置。
Ctrl+Enter	| 在下一行插入新行。举个栗子：即使光标不在行尾，也能快速向下插入一行。
Ctrl+Shift+Enter	| 在上一行插入新行。举个栗子：即使光标不在行首，也能快速向上插入一行。
Ctrl+Shift+[	| 选中代码，按下快捷键，折叠代码。
Ctrl+Shift+]	| 选中代码，按下快捷键，展开代码。
Ctrl+K+0	| 展开所有折叠代码。
Ctrl+←	| 向左单位性地移动光标，快速移动光标。
Ctrl+→	| 向右单位性地移动光标，快速移动光标。
shift+↑	| 向上选中多行。
shift+↓	| 向下选中多行。
Shift+←	| 向左选中文本。
Shift+→	| 向右选中文本。
Ctrl+Shift+←	| 向左单位性地选中文本。
Ctrl+Shift+→	| 向右单位性地选中文本。
Ctrl+Shift+↑	| 将光标所在行和上一行代码互换（将光标所在行插入到上一行之前）。
Ctrl+Shift+↓	| 将光标所在行和下一行代码互换（将光标所在行插入到下一行之后）。
Ctrl+Alt+↑	| 向上添加多行光标，可同时编辑多行。
Ctrl+Alt+↓	| 向下添加多行光标，可同时编辑多行。

#### 编辑类
快捷键 | 功能
------------ | -------------
Ctrl+J	 | 合并选中的多行代码为一行。举个栗子：将多行格式的CSS属性合并为一行。
Ctrl+Shift+D	 | 复制光标所在整行，插入到下一行。
Tab	| 向右缩进。
Shift+Tab |	向左缩进。
Ctrl+K+K	| 从光标处开始删除代码至行尾。
Ctrl+Shift+K	| 删除整行。
Ctrl+/	| 注释单行。
Ctrl+Shift+/	| 注释多行。
Ctrl+K+U	| 转换大写。
Ctrl+K+L	| 转换小写。
Ctrl+Z	| 撤销。
Ctrl+Y	| 恢复撤销。
Ctrl+U	| 软撤销，感觉和	Gtrl+Z	一样。
Ctrl+F2	| 设置书签
Ctrl+T	| 左右字母互换。
F6	| 单词检测拼写

#### 搜索类
快捷键 | 功能
------------ | -------------
Ctrl+F	| 打开底部搜索框，查找关键字。
Ctrl+shift+F	| 在文件夹内查找，与普通编辑器不同的地方是sublime允许添加多个文件夹进行查找，略高端，未研究。
Ctrl+P	| 打开搜索框。举个栗子：1、输入当前项目中的文件名，快速搜索文件，2、输入@和关键字，查找文件中函数名，3、输入：和数字，跳转到文件中该行代码，4、输入#和关键字，查找变量名。
Ctrl+G	| 打开搜索框，自动带：，输入数字跳转到该行代码。举个栗子：在页面代码比较长的文件中快速定位。
Ctrl+R	| 打开搜索框，自动带@，输入关键字，查找文件中的函数名。举个栗子：在函数较多的页面快速查找某个函数。
Ctrl+：	| 打开搜索框，自动带#，输入关键字，查找文件中的变量名、属性名等。
Ctrl+Shift+P	| 打开命令框。场景栗子：打开命名框，输入关键字，调用sublime	text或插件的功能，例如使用package安装插件。
Esc	| 退出光标多行选择，退出搜索框，命令框等。

#### 显示类
快捷键 | 功能
------------ | -------------
Ctrl+Tab |	按文件浏览过的顺序，切换当前窗口的标签页。
Ctrl+PageDown |	向左切换当前窗口的标签页。
Ctrl+PageUp |	向右切换当前窗口的标签页。
Alt+Shift+1 |	窗口分屏，恢复默认1屏（非小键盘的数字）
Alt+Shift+2 |	左右分屏-2列
Alt+Shift+3 |	左右分屏-3列
Alt+Shift+4 |	左右分屏-4列
Alt+Shift+5 |	等分4屏
Alt+Shift+8 |	垂直分屏-2屏
Alt+Shift+9 |	垂直分屏-3屏
Ctrl+K+B |	开启/关闭侧边栏。
F11 |	全屏模式
Shift+F11 |	免打扰模式
