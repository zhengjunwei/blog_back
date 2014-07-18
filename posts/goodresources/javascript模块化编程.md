---
title:js模块化编程学习
layout: page
category: js
date: 2014-06-14
---

Javascript模块化编程
理想的情况下，开发者只需要实现核心的业务逻辑，其他都可以加载到别人已经写好的模块。
原始写法
function m1(){
	
	//
}

function m2(){
 //
}

上面的做法有一个缺点，污染了全局变量，无法保证 不与其他模块发生变量名冲突，而且模块成员之间看不出直接的关系。

所以，可以写成下面的方法 
var module1 = new Object({
	_count:0,
	m1:function(){

	},
	m2:function(){

	}
});

但是上面的写法还是有点不好的地方，所有的变量我们都暴露给了调用者

为了达到保护私有属性和方法的目的，可以使用IIFE

var module1 = (function(){
	var _count = 0;
	var m1 = function(){

	}

	var m2 = function(){

	}

	return {
	  m1:m1,
	  m2:m2
	}
})

放大模式
如果一个模块很大，必须分成几个部分，或者一个模块需要继承其他的模块，这时就有必要采用放大模式
var module1 = (function(mod){
	mod.m3 = function(){};
	return mod;
})(module1)

宽放大效果
在浏览环境中，模块的各个部分通常都是从网上获取，有时无法知道哪些部分会先加载，如果采用上一节的写法，第一个执行的部分有可能加载一个不存在的空对象，这时，就需要用到宽放大模式。

var module1 = (function (mod){
	return mod;
})(window.module1||{});

与放大模式相比，宽放大模式就是IIFE的参数可以是空对象。

输入全局变量
独立性是模块的重要特点，模块内部最好不要与程序的其他部分进行交互。
为了在模块内部调用全局变量，必须显式的将其他变量输入模块

var module1 = (function($,YAHOO){
	
})(jQuery,YAHOO);
上面的Module1模块需要用到jQuery库，YUI库，就把这二个库当作参数输入。

模块的规范
有了模块，我们就可以更方便的使用别人的代码，想要什么功能，就加载什么模块。
但是，这样做有一个前提，就是大家必须以同样的方式编写模式，否则你有你的写法，我有我的写法，这样就乱套了。
CommonJS
node.js的模块系统，就是参照CommonJS规范实现的，在CommonJS中，有一个全局性方法require(),用于加载模块，假定有一个数学模块math.js，就像下面这样加载。
var math = require('math');
然后就可以调用模块提供的方法:
var math = require('math');
math.add(2,3);//5
我们只要知道，require()用于加载模块就行了
浏览器环境
有了服务器端模块之后，很自然的，大家就想到了客户端模块，而且最好两者能够兼容，一个模块不用修改，在服务器和浏览器都可以运行。
但是，由于有一个重大的局限，使得CommonJS规范不适用于浏览器环境。
比如
var math = require('math');
math.add(2,3);
如果var math = require('math')加载时间太长，整个应用就会假死
因此，浏览器端不能采用同步加载 ，只能采用异步加载，这就是AMD规范产生的背景。
AMD：Asynchronous Module Definition，异步模块定义
语法
require([module],callback)
第一个是数组，里面的成员就是要加载的模块，第二个是callback,则是加载成员之后回调的函数。
require(['math'],function(math){math.add(2,3)});
我们来看一下实现了这个规范的库,require.js
为什么要用require.js?
我们以前的代码是这样写的
<script src="1.js"></script>
<script src="2.js"></script>
<script src="3.js"></script>
<script src="4.js"></script>
这样的写法有一个很大的缺点，首先，加载的时候，浏览器会停止网页渲染，加载文件越多，网页失去响应的时间就会越长
其次，由于js文件之间存在依赖关系，因此必须严格保证加载顺序，依赖性最大的模块一定要放到最后加载，当依赖关系很复杂的时候，代码的编写和维护都变得困难。

require.js的作用：
(1):实现js文件的异步加载，避免网页失去响应；
(2)：管理模块之间的依赖性，便于代码的和维护

<script src="js/require.js"></script>
有人会说，加载这个文件，也有可能会造成网页失去响应，一个办法是放在底部，另一个办法是这样:
<script src="js/require.js" defer async="true"></script>
defer为了支持IE的
下面我们就可以写自己的代码了
可以这样写
<script src="js/require.js" data-main="js/main"></script>
上面的main.js我们称之为主模块，意思是整个网页的入口代码，它有点像c语言的main函数，所有的代码都从这儿开始运行。
如果我们的代码不依赖任何模块，我们可以直接这样写Js代码。
//main.js
alert('加载成功');
但这样的话，就没有必要使用require.js了
真正常见的情况是，主模块依赖于其他模块，这时就要使用AMD规范定义的require函数了。

//main.js
require(['moduleA','moduleB','moduleC'],function(moduleA,moduleB,moduleC){
	///
});

require接受二个参数，第一个参数是一个数组，表示依赖的模块。

require(['jquery','underscore','backbone'],function($,_,Backbone){
	//
});

模块的加载
上一节最后的示例中，主模块的依赖模块是['jquery','underscore','backbone']，默认情况下，require.js
假定这三个模块与main.js同一个目录，文件名分别是jquery.js,underscore.js,backbone.js
然后自动加载
使用require.config()方法，我们可以对模块的加载行为进行自定义，require.config就写在主模块的头部
require.config({
	paths:{
	  "jquery":'jquery.min',
	  "undescore":'underscore.min',
	  "backbone":'backbone.min'
	}
});

如果这些文件目录和main.js不在同一目录，可以这样写

require.config({
	paths:{
	  "jquery":'lib/jquery.min',
	  "underscore":"lib/underscore.min",
	  "backbone":"lib/backbone.min"
	}
});
另一个是直接改变基目录
require.config({
	"jquery":"jquery.min",
	"underscore":"underscore.min",
	"backone":"backone.min"
});

如果是在另一个主机上，可以这样写
require.config({
	path:{
	  "jquery":
	  "http://wfewef"
	}
})
require.js的要求，每个模块是一个单独的文件，这样的话，如果加载多个模块，就会发出多次HTTP请求，会影响网页的加载速度，因此，require.js提供了一个优化工具，当模块部署完成后，可以用这个工具将多个模块合并在一个文件里，减少HTTP请求数。

AMD模块的写法
require.js加载的模块，采用AMD规范，也就是说，模块必须按照AMD的规范来写。
具体来说，就是模块必须采用特定的define函数来定义，如果一个模块不依赖其他模块，那么可以直接定义在define函数之中。
//math.js
define(function(){
	var add = function(x,y){
	  return x + y;
	};
	return {
	  add:add
	};
});
加载方式如下：
require(['math'],function(math){alert(math.add(1,1))})

define(['myLib'],function(myLib){
	function foo(){
	  myLib.dosomething();
	}
	return {
	  foo:foo
	};
});

加载非规范的模块
理论上，require.js加载的模块，必须是按照AMD规范，用define函数来定义的模块，但是实际上虽然有一部分流行的
函数库符合AMD规范像jQuery,但更多的库不符合，那么，require.js是否能够加载非规范的模块呢？

回答是可以的。
这样的模块在用require()加载之前，要先用require.config()方法，定义他们的一些特性。
比如：underscore backbone这二个库，都没有采用AMD，如果要加载它们，可以先定义他们的一些特性。

require.config({
	shim:{
	  'underscore':{exports:'_'},
	  'backbone':{
	    deps:['underscore','jquery'],
	    exports:'Backbone'
	  }
	}
});

require.config()接受一个配置对象，这个对象除了有前面说过的paths属性之外，还有一个shim属性，专门用来配置不兼容的模块。

具体来说，每个模块要定义1，exports值，输出的变量名，表名这个模块外部调用时的名称2,deps数组，表示这个模块的依赖性。
比如jQuery插件可以这样写
shim:{
	'jquery.scroll':{
	  deps:['jquery'],
	  exports:'jQuery.fn.scroll'
	}
}

require.js的插件

domready插件，可以让回调函数在页面DOM结构加载完成后再运行
require(['domready',function(doc){
	 ///
}]);

text和image插件，允许require.js加载文件和图像文件
define([
  'text!review.txt',
  'image!cat.jpg'
],
function(review,cat){
	console.log(review);
	document.body.appendChild(cat);
}
)
类似的还有json,mdonw，用于加载json文件和markdown文件

