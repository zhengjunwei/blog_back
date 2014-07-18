---
title: Javascript SVG parser and renderer on Canvas
layout: page
category: canvas svg
date: 2014-06-06
---

我前面有一个文章是兼容各种浏览器取得某元素的色值的，[RGB color parser in JavaScript](http://zhengjunwei.com/post/rgb-color-parser-in-javascript),我们现在介绍的这个就依赖这个取色值的工具[点这里下载](http://www.phpied.com/files/rgbcolor/rgbcolor.js)。

##about
canvg是一个SVG解析和渲染器，它接受SVG文件或者SVG文件的内容，通过js解析，把结果渲染成canvas元素。

##用法
+ 包含下列文件

```
<script type="text/javascript" src="http://canvg.googlecode.com/svn/trunk/rgbcolor.js"></script> 
<script type="text/javascript" src="http://canvg.googlecode.com/svn/trunk/StackBlur.js"></script>
<script type="text/javascript" src="http://canvg.googlecode.com/svn/trunk/canvg.js"></script> 
```
+ 在你的html里放一个canvas元素
 
```
<canvas id="canvas" style="width:1000px;height:600px;"></canvas>
```

+开始使用

```
<script>
	window.onload = function(){
		canvg('canvas','../path/to/your.svg');

		//or

		canvg(document.getElementById('drawingArea'),'<svg>....</svg>')

		//or

		canvg('canvas','file.svg',{ignoreMouse:true,ignoreAnimation:true})
	}
</script>