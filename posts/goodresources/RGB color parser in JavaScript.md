---
title: RGB color parser in JavaScript
layout: page
category: Javascript
date: 2014-06-06
---
# RGB color parser in JavaScript

可以到这里来体验一把[color parser demo](http://www.phpied.com/files/rgbcolor/rgbcolor.html "color parser demo")
在firefox的格式是rgb(xxx,yyy,zzz),在IE中是#0000的格式
为了解决什么问题？作者这样说的：
In FireFox when you get a computed style, it's in the format rgb(xxx, yyy, zzz)
In IE, it's #xxyyzz.
So I needed to parse both formats.
不过，我在IE8，firefox29.0.1,chrome35.0.1916.114 m里面都是得到的rgb()这种格式的，应该是为了兼容老的版本吧

```
	var div = document.createElement('div');
	div.className = 'myDiv';//.myDiv{background:rgb(255.187.0);width:200px;height:200px;}
	document.body.appendChild(div);
	if(window.getComputedStyle){
		console.log(window.getComputedStyle(div)['backgroundColor']);
	}else{
		console.log(div.currentStyle['backgroundColor']);
	}
```

##如何使用
使用这个构造函数RGBColor()

```
var color = new RGBColor('darkblue');
if(color.ok){//ok代表是对的
	//alert channels
	alert(color.r + ',' + color.g + ','+ color.b);
	alert(color.toHex());
	alert(color.toRGB());
}
```
##如何工作的?
* The class accepts a string
* Any leading # is stripped; spaces are stripped
* There' s a check against a list of valid color names, such as "red" and "darkorange" and these are mapped to a Hex code
* An array of objects is defined that have one regexp property and a function that knows what to do if the regexp find a match
* There is a quick validation that the channel values are between 0 and 255
* The two getters are defined - toHex() and toRGB()
* Finally there is a function that acts as both a self-documentation and self-uinttest, where a bunch of accepted values are automatically run through the class and parsed and the result is displayed as a help text.

