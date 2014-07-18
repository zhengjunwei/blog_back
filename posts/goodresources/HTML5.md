---
title:html5
layout: page
category: others
date: 2014-06-07
---
#html5
做pc端，对移动端进行适配
不同的布局方法对移动端的适配能力不一样
手机上的一个像素可以理解成PC上的2个像素
640左右(宽度)像素，往上适配容易，往下适配也不失真
对字体操作最好不要用像素单位，pt,em
PC端单张图片不要超过80k
移动端 尽量少用float
###常用的css归纳
-webkit-text-size-adjust:none;这一行放到reset样式表上面
-webkit-user-select:none;
转变转变盒模型(padding border-width,不含margin)
-webkit-box-sizing:border-box
色值少的用png8
色值多的用jpg

json查看器 结构+数目
正常样式表最好不要超过300行

使用jQuery的时候最好不要用opacity,.sibling，会耗性能