---
date: 2014-06-26
---
下面这本书的所有内容来自于ng-book
翻译原则:尽量按理解的来翻译

什么是AngularJS
ng是一款来自google的前端js框架，它的特点就是快速和容易。
ng的一些特点:
- 逻辑、数据模型和视图分离(MVC)
- ajax
- 依赖注入
- 浏览器历史(使书签，前进后退按钮工作起来像普通的web app)
- 单元测试更加容易
- 语义化自定义标签
- 自动化双向数据绑定

## 第一个例子Hello World

```
<!doctype html>
<html ng-app>
<head>
	<title></title>
	<script type="text/javascript" src="angular/angular-1.0.1.min.js"></script>
</head>
<body>
  <!----你可以在input里面输入world，然后就出现了hello world--->	
  <input ng-model="name" type="text" placeholder="Your name">
  <h1>Hello {{name}}</h1>
</body>
</html>
```


当ng认为某个值改变了的时候，它会调用$digest来检查脏值，同样的，当ng运行的时候，它会不停的检查这些值的改变没有。这个过程被称为脏值检查，脏值检查是一个相对有效的方法来检查模型的改变，每次只要可能有改变，ng将会进行一个event loop(类似原生的js),来确定一下所有的值有没有发生改变。

(下面二段其他框架的特点来自司徒正美)
- knockout:最早冒出来的JS MVVM库，通过转换VM中所有要监听的东西为函数，然后执行它们，得到某一时刻中，一共有多少函数被执行，将它们放到栈中，最底的就是最先被执行的，它上面的就是此函数所依赖的函数，从而得到依赖关系。 然后设计一个观察者模式，从上面的依赖检测中，将依赖函数作为被依赖者（最先执行的那个的）的订阅者，以后我们对被依赖者进行赋值时，就会通先订阅者更新自身，从而形成一个双向绑定链。 并且，knockout会将视图中的绑定属性进行转换，分解出求值函数与视图刷新函数，视图刷新函数依赖于求值函数，而求值函数亦依赖于我们VM中的某些属性（这时，它们都转换为函数），在第一次扫描时，它们会加入对应属性的订阅者列队中， 从而VM中的某个属性改变，就会自动刷新视图。 
评价：实现非常巧妙，是avalon0.1-0.3的重要学习对象，但将属性变成一个函数，让人用点不习惯，许多用法都有点笨笨的。 虽然是一个轻盈的库，但扩展性不强，里面的实现异常复杂，导致能参与源码的人太少。

- emberjs: 一个大而全的框架，包罗万象。一开始是使用Object.defineProperty+观察者实现，但IE8的问题，让它不得不启用上帝setter, 上帝getter。没有自动收集依赖的机制，没有监控数组，计算属性需要自己指定依赖。VM可继承。 VM与视图的双向绑定依赖于其强大无比上万行的Handlebars 模板。听说是外国目前最好用的MV*框架。因为作者既是jQuery的核心成员，也是Rails的核心成员，虽然由于技术能力没实现自动收集依赖，但框架的其他方面做得非常易上手，人性化。 
评价：太大了，优缺点同python的Django框架。


## 好吧，我们开始使用controller了

```
<!doctype html>
<html ng-app>
<head>
	<title></title>
	<script type="text/javascript" src="angular/angular-1.0.1.min.js"></script>
</head>
<body>
  <div ng-controller="MyController">	
  <h1>{{clock}}</h1>
  </div>
  <script type="text/javascript">
    function MyController($scope){
    	var updateClock = function(){
    		$scope.clock = new Date();
    	};
    	setInterval(function(){
    		$scope.$apply(updateClock)},1000
    	);
    	updateClock();//
    }
  </script>
</body>
</html>
```

上面虽然可以实现我们想要的效果，但是最佳实践是在视图中用一个对象的属性来作为ng表达式，像下面这样

```
<!doctype html>
<html ng-app>
<head>
	<title></title>
	<script type="text/javascript" src="angular/angular-1.0.1.min.js"></script>
</head>
<body>
  <div ng-controller="MyController">	
  <h1>{{clock.now}}</h1>
  </div>
  <script type="text/javascript">
    function MyController($scope){
    	var updateClock = function(){
    		$scope.clock = {
    			now:new Date()
    		};
    	};
    	setInterval(function(){
    		$scope.$apply(updateClock)},1000
    	);
    	updateClock();//
    }
  </script>
</body>
</html>
```

## 把我们的控制器关进全局变量的笼子里

众所周知，把我们的变量暴露在全局是不科学的，下面我们将使用模块式编程方法，把控制器放到相对应的一个全局单例对象下面

这样做的好处:
- 保持全局命名空间干净
- 由于功能的独立，使得测试更加的easy
- 使应用程序之间共享代码更加容易
- 使我们的app可以按场景加载不同的代码段

```
angular.module('myApp')
```

我们观察上面的这段代码，这是一个getter方法，我们通过这个可以得到一个ng的模块。

## scopes

Scopes是ng app的核心功能，我们整个框架都会用到这个框架，所以知道它们的工作机制是很重要的。
Scopes是app的状态的先知，我们靠着我们的动态绑定，当view改变的时候，我们可以通过$scope来快速
更新我们的$scope,同理，当$scope改变的时候，我们也可以动态的改变我们的view。
当一个ng的app运行的时候，会在ng-app的元素上绑定我们的$rootScope,这个是所有的$scope对象的根scope
$rootScope是我们ng里面作用域最大的变量，类似于我们在原生的js里面最好不要脏了全局环境，我们也最好不要在这一层作用域绑定太多的逻辑。
$scope是一个原生的js对象，我们可以给它添加对象或方法按我们的要求。
所有的$scope的属性都可以被view共享，你可以把scopes当作是view models，比如:

```
<!doctype html>
<html>
<head>
	<title></title>
	
</head>
<body>
<div ng-app="myApp">
  <h1>Hello {{name}}</h1>
</div>
<script type="text/javascript" src="angular/angular-1.0.1.min.js"></script>
  <script type="text/javascript">
    angular.module('myApp',[]).run(function($rootScope){
    	$rootScope.name = "world";
    })  
  </script>
</body>
</html>
```

我们上面讲过，把变量绑定在$rootScope当下这个作用域是不好的，所以，我们改造一下

```
<!doctype html>
<html  ng-app="myApp">
<head>
	<title></title>
	
</head>
<body>
<div ng-controller="myController">
  <h1>Hello {{name}}</h1>
</div>
<script type="text/javascript" src="angular/angular-1.0.1.min.js"></script>
  <script type="text/javascript">
    angular.module('myApp',[]).controller('myController',function($scope){
    	$scope.name = "world";
    })  
  </script>
</body>
</html>
```

当你创建一个指令或者一个控制器的时候，ng将会通过$injector来注入一个新的scope
当$scope链接到view的时候，所有的创造$scope的指令将会注册他们的watches到它们的父scope
通过$digest环，所有的位于$rootScope下scope都会进行脏值检查

ng表达式:
- 所有的表达式都可以访问到本地的$scope的为量
- 如果发生类型错误或者引用错误，不会报错
- 不能有if,else之类的控制语句
- 可以接受filter,或者filter链

尽管你的ng app会自动的运行$digest loop环，有时候你还是需要手动的来解析ng 表达式

```
angular.module('emailParser',[]).config(['$interpolateProvider',function($interpolateProvider){
	$interpolateProvider.startSymbol('__');
	$interpolateProvider.endSymbol('__');
}]).factory('EmailParser',['$interpolate',function($interpolate){
	return {
	  parse:function(text,context){
	    var template = $interpolate(text);
	    return template(context);
	  }
	}
}])
``` 

自定义过滤器

```
angular.module('myApp.filters',[]).filter('capitalize',function(){
	return function(input){}
})
```

```
 angular.module('myApp.filters',[]).filter('capitalize',function(){
    //这里的input接受传进来的值，类似于Linux里面的管道
	return function(input){
	  if(input)
	    return input[0].toUpperCase()+input.slice(1);
	}
 });
```

```
angular.module('validationExample',[]).directive('ensureUnique',function($http){
	return{
	  require:'ngModel',
	  link:function(scope,ele,attrs,c){
	    scope.$watch(attrs.ngModel,function(){
	      $http({
		    method:"POST",
		    url:"/api/check/"+attrs.ensureUnique,
		    data:{'field':attrs.ensureUnique}	
	      }).success(function(data,status,headers,cfg){
			 c.$setValidity('unique',data.isUnique);
	      }).error(function(data,status,headers,cfg){
		    c.$setValidity('unique',false);
	      })
	    })
	  }
	}
})
```


