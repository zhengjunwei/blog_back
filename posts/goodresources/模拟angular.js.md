---
title:模拟angular.js
layout: page
category: angular.js
date: 2014-06-15
---
```

<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>Document</title>
</head>
<body>
	<div style="width:300px;height:200px;background: green;">
		wgeorgjer
	</div>
<script>
var angular = function(){};//定义了一个angular全局方法

Object.defineProperty(angular,"module",{//给angular这个Function类型的对象添加一个module的属性，第三个参数是module这个属性的属性值
  value:function(modulename,args){//下面的demo传进来的是Test这个参数
    var module = function(){//局部变量，这里是定义了一个构造函数module
      this.args = args;
      this.factoryObject = {};
      this.controllerObject = {};
     // console.log(this);//module {args: undefined, factoryObject: Object, controllerObject: Object, factory: function, controller: function}
 
    }
    //console.log(this);//这里的this是  function (){} 
    module.prototype.factory = function(name,service){
      //if service is not a function ...  
      //if service() the result is not a object ... and so on
      this.factoryObject[name] = service();
    }
    module.prototype.controller = function(name,args){
      var _self  = this;
      //init
      var content = {
        $scope:{},
        scope:function(){
          return content.$scope;
        }
      //  $someOther:{...}
      }

      var ctrl = args.pop();
      console.log(typeof ctrl);//function
      var factorys = [];
      while(service = args.shift()){
        if(service in content){
          factorys.push(content[service])
        }else{
          factorys.push(_self.factoryObject[service])
        }
        
      }
      ctrl.apply(null,factorys);

      _self.controllerObject[name] = function(){
        return content;
      };
    }
    var m = new module();//创造了一个构造函数是module的对象
    window[modulename] = m;//把modulename这个传进来的参数作为全局window的一个属性
    return m;
  }

  
})
</script>	
<script>
	var hello = angular.module('Test');
//console.log(hello);
hello.factory("actionService",function(){
	var say = function(){
		console.log("hello")
	}
	return {
		"say":say
	}
})

hello.controller("doCtrl",['$scope',"actionService",function($scope,actionService){
	$scope.do = function(){
		actionService.say();
	}
}]);

hello.controllerObject.doCtrl().scope().do()
</script>
</body>
</html>

```