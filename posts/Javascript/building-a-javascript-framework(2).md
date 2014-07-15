## 动手建立一个javascript框架(1)

这本书可以指导你构建一个js的框架，它将教会你如何构建一个在真实世界中能够使用的像jQuery一样的框架。
一路下来，我们将探索一些构成现代js程序的一些基础
	- 浏览器功能检测
	- 干净和可复用的API设计
	- 基准测试和性能测试
	- 编写优雅的js代码
	- 使用Github
我们将称我们未来马上要构建的框架为turing.js

### 框架风格
如果你参加过开源或者内部的活动，你会懂得如何与众多的人一起工作，一开始对目标的确认和开发风格的确定是很重要的。
- 语义化:变量和方法名应用让人容易理解它的作用功能是什么
- 适配性：兼容各个浏览器
- 代码易懂
- 注释清晰:加上TODO和FIXME之类
- Simple:保持代码简单，不要让读者觉得枯燥
- 缩进:两列缩进
- 分号
- 代码质量评测:用jsLint
- 测试:通过浏览器和console
- 版本控制

### 高逼格的框架结构
第一个我们要问的问题是，一个js框架的结构应该是什么样子的？在2005年兴起了Ajax和黄渐变技术(“Yellow Fade Technique”的效果，正式我们这次所需要的。这个效果是使用指定的开始颜色高亮指定的对象，然后渐渐消退，直到到达终了颜色结束。并使得指定对象的背景色恢复到高亮前的状态。)，于是人们纷纷开始使用一些类库来使这些技术的使用变得容易，现在到了2010年，我们编写服务器端的js代码,并且创造了复杂的前端行为。我们现在再也不用为了使用框架而记住一堆的命名空间了。
我们可以看一下比较稳定的prototype.js库，它把很多原生的对象的原型进行了修改，并且提供了许多高层的对象。BBC为了避免变量污染，专门设计了Glow，Glow通过命名空间的手段来防止变量的污染。当你使用Prototype的时候，有时候你会觉得不可思议，因为Prototype的目的就是为了简化浏览器端的开发难度。Glow则提供了一系列像glow.lang.toArray这样的工具函数.
译者注:BBC Glow介绍
  Glow的特点:简化DOM操作，事件处理和动画等，有一系列的UI组件，清晰和完善的文档，对浏览器兼容的支持

```
	
	glow.ready(function() {
	    var p = glow.dom.get("#greeting");
	    p.html("hello, world");
	});



	glow.ready(function() {
        glow.dom.get("p").css("border", "2px dashed #CCC");
        
        glow.events.addListener(
                ".salutation",
                "click",
                function () { 
                        alert(glow.dom.get(this).html()); 
                }
        );
	});

	
```
`




通过这个课程的学习，你将会更加容易的使用其他的一些框架，为了更好的让库发挥作用，我们还会让我们编写的库可以进行参数的配置.

我们开发的框架将会有点类似Glow,我们应该能够区分得开哪些功能API是浏览器提供的，哪些功能是CommonJS提供的.

另一个关于prototype.js有意思的地方是，它可以快速的定义一个结构优良的可以复用的的类，比如Object.extend和Class,然后我们可以复用这些来构建我们的基础库:


```

  var Hash = Class.create(Enumerable,(function(){
  	function initialize(object){
  		this._object = Object.isHash(object)?object.toObject():Object.clone(object);
  	}
  }))


function clone(object){
	  return extend({},object)
	}

	对于Hash本质上是:
	function clone(){
	  return new Hash(this);
	}




```

译者注:上面的代码解读为：如果object是一个普通对象，那么直接调用Object.clone方法，如果是Hash实例，调用object.toObject，
Hash是Prototype作者扩展出来的一个数据类型，本质上是一个普通的js对象，Hash是作者自己创建的一个数据类型，因此,Hash采用的方式是Class.create的方式。每个Hash对象内部会维护一个内部的对象_object,通过get,set,unset来访问
复制当前的hash对象用clone,克隆内部的对象_object用toObject


###　Helper方法

MooTools，JQuery,Prototype都定义了help方法来供使用:


```

	  //Prototype
	  function $H(object){
	  	return new Hash(object);
	  }
	
	  //MooTools
	  function $H(object){
	  	return new Hash(object);
  }


```

在框架中包含help方法是一个很明智的选择但因为我们的turing.js主要目的是为了学习，所以我们只会包含一些简明但是清晰的heler方法。

比如当你教别人用jQuery的时候，他们会不会意识到其实$()并不是原生的方法？

### 初始化

Mootools和Prototype使用字面量的方式来调用，而jQuery和Glow采用了另外一种方法

```

  var Prototype = {
    Version:'1.1'
  }

  var MooTools = {
    'version':'1.2.dev',
    'build':''
  }

  (function(window,undefined){
    var jQuery = function(selector,context){
    	return new jQuery.fn.init(selector,context);
    },
    ...
    jqueryu:"VERSION"
	}

	window.jQuery = window.$ = jQuery;
  })(window);



```


Glow 和 jQuery都是通过使用匿名函数的，并且把它们自身暴露给了window对象，我们的turing.js里面也将采用这种方法.

### 模块化和插件

jQuery,MooTools和Glow都是模块化的优秀体现，同样的，我们也将会使用这种机制，比如我们可以把文件结构组织成:
	* turing.core.js
	* turing.functional.js

### 开始编程

我们将使用我们的riot.js来写我们的单元测试，因为它是如此的纯净和小巧.

也许你会以为测试一个library framework是很无聊无意义的事情，但是我们必须要保证他在各种环境中能正常使用，我们将使用Rhino和Firefox测试

核心测试将包括下面这些：
  + turing能正确的实例化
  + turing可以有一些属性让我们知道他的情况，比如版本号之类的

 测试代码像这样:

 ```
	 Riot . context ( 'turing.core.js' , function () {
	given ( 'the turing object' , function () {
	should ( 'be global and accessible' , turing ). isNotNull ();
	should ( 'return a VERSION' , turing . VERSION ). isNotNull ();
	should ( 'be turing complete' , true ). isTrue ();
	});
	});
```

上面的测试代码和我们的库代码合并后，我们会得到这样的代码：

```
 (function (global){
    var turing = {
      VERSION:"0.0.1",
      lesson:'Part 1:Library Architecture'
	};

	if(global.turing){
		throw new Error('turing has already been defined');
	}else{
		global.turing = turing;
	}
  })(typeof window === 'undefined'?this:window);

它将这样工作:

```

  js > load('turing.core.js');
  js > print(turing.VERSION);
  

```

