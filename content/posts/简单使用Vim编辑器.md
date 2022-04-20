---
title: "简单使用Vim编辑器"
categories: [ "默认" ]
tags: [ "Vim" ]
draft: false
slug: "3"
date: "2021-03-28 12:00:00"
---

Vim是从vi发展出来的一个文本编辑器。提供代码补完、编译及错误跳转等功能

vim的三种模式

正常模式：Esc或Ctrl+[进入
插入模式：按i键进入
可视模式：按v或者V，ctrl-V（或者ctrl-Q）进入

v 进入字符可视化模式
V 进入行可视化模式
Ctrl+v 进入块可视化模式

可视模式是用来选取一段文本，光标移到段首，在普通模式下按 v 进入可视模式，然后把光标移到段末




vim hallo.js

在vim界面新建文件

:open hallo.js

在vim界面打开一个新窗口新建文件

:split file

切换到上一个文件

:bp

切换到下一个文件

:bn

查看vim文件列表

:args

打开远程文件

:e \\hallo abc.txt

/text：按n健查找下一个，按N健查找上一个

?text：按n健查找上一个，按N健查找下一个

:wq：保存并且退出，加!表示强制

:q!：强制退出并且放弃所有更改


:e!：放弃修改，并且重新打开文件
