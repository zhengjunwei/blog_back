关于我的博客
====
之前的博客现已换到github上来了

这是我的博客 [郑俊伟的博客](http://blog.zhengjunwei.com/)，数据都托管在[GitHub](https://github.com/)。
所有的文章都是用`MarkDown`写的。

基于gitpress搭了一个懒人博客。

搭建方式见月影博客 [gitpress 基于 github 的懒人博客系统](http://blog.silverna.org/~posts/gitpress/2013-11-17-gitpress.org%20%E5%9F%BA%E4%BA%8Egithub%E7%9A%84%E6%87%92%E4%BA%BA%E5%8D%9A%E5%AE%A2%E7%B3%BB%E7%BB%9F.md)。



近期要研究的库
zepto,turing.js,underscore,sugar,vue.js(这个是MVVM的),spine.js(作者有书)

再谈谈兼容性这块，不同的开发公司对这个都有不太一样的需求，一般都要求能兼容IE8+（需要兼容IE6/7？可怜的娃请你备好阿司匹林）。个人建议一切效果均以最新的浏览器上所显示的优先（IE取消兼容模式），可在页面头部加入<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">


How time go past! i wish i will code everyday with the lovely javascript.i want to prove my value to the world !
世界上最牛的几个js程序员
http://code.tutsplus.com/articles/30-developers-you-must-subscribe-to-as-a-javascript-junkie--net-18151


http://javascriptshow.com/episodes/52


http://code.tutsplus.com/articles/120-tips-tricks-and-tuts-from-2009-worth-your-time--net-8298

http://james.padolsey.com/posts/
关于SVG的PPT:https://docs.google.com/presentation/d/1CNQLbqC0krocy_fZrM5fZ-YmQ2JgEADRh3qR6RbOOGk/preview?sle=true#slide=id.g3336a6aae_0262


提高自己的好办法:多写代码，并且把自己写的代码暴露出来，让别人挑错！！！！！一定要多写多写


关于正则最好的网址 http://www.regexr.com/


网页版 markdow书写工具：http://epiceditor.com/
_background:red;              只有ie6认得
+background:red;              ie6/7可认
*background:red;              ie6/7可认
background:red\0;             ie8+
background:red\9;             ie皆可认
background:blue!important;    除了ie6都认得(包括其它浏览器),如果important跟重名属性不在一个区域则ie6也支持)
:root .ww{background: #000;}  除ie9+认得(包括其它浏览器)
*html .ww{background: #000;}  只有ie6认得
*+html .ww{background: #000;} 只有ie7认得

@media screen and (-webkit-min-device-pixel-ratio:0) {}
仅webkit能认


fade时，要png背景不能为本身元素。否则ie会有黑背景..内部是png超出外转边界也会出现黑背景且开始时显示外围内的内容。到全显示的时候一下再显示出来

ie8中用js创建的元素不能于:first-letter。要不然会卡死
