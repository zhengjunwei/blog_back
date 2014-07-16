## 动手建立一个javascript框架(3)

## 面向对象的Javascript

不是所有的js framework都会提供类，Douglas Crokford 讨论了经典的基于类的继承的对象模型，在js中使用继承是一个很受关注的话题，稍晚些，他又指出了基于原型的继承其实也很强大，并不比基于类的逊色.

为什么js库会提供类，因为很多有其他语言背景的人会把其他语言的特性想引入js，像Prototype就是受Ruby的灵感.

这一章节，我们将解释基于原型的继承和OO，并且开始创建基于OO的js.我们将使用它来构建我们的turing.js.

### Object,Classes VS. Prototype Classes

基于原型的类像这样

```
		  function Vector(x,y){
		    this.x = x;
		    this.y = y;
		  }

		  Vector.prototype.toString = function(){
		    return 'x:'+this.x + 'y:'+this.y;
		  }

		  v = new Vector(1,2);



		  function A(x,y){
			this.b = x;
			this.c = y;
		}

		function D(){
			A.apply(this,arguments);
			this.d = 4;

		}

		D.prototype = new A;
		D.prototype.constructor = D;

		p = new D(1,2);
		console.log(p)

```


### 原型 VS. 类

有很多种模式可以处理原型的继承，我们抽象出来一个类是很有用的，定义一个类的API，可以让我们的保存更加的简洁并且人们阅读的时候也更加的愉快.










