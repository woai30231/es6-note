### 简介

* **什么是箭头函数（arrow-function）?**在es6中，箭头函数就是你在es5中所写函数形式的一种简短形式！简而言之，跟你以前写过的函数主要有两个区别：一、书写方式不同（外形上的不同）；二、未绑定this、arguments等。箭头函数特别适合那种没有严格绑定this、arguments等的函数！

* 箭头函数在语法上主要有以下几种方式：

```javascript
	(param1,param2,...,paramN) => { statements }
	(param1,param2,...,paramN) => expression
	//等价于：(param1,param2,...,paramN) => {return expression;}
	//这里注意statements和expression的区别：一个是语句，一个是表达式
	//"x + 5"是表达式，而"x = x + 5;"语句
	//如果是一个表达式，那么函数的返回值就是表达式计算的值，如果是语句，函数体需要大括号
	//语句的返回值由函数体中return关键字决定

	//当函数只有一个参数的时候，圆括号可要可不要，如
	(param1) => {statements}
	param1 => {statements}

	//但是当函数没有参数的时候，这个圆括号一定要，如
	() => {statements}
	() => expression


	//这里再多说一句，如果箭头函数在第二种形式中返回表达式是一个对象，那么请记住用一个圆括号把对象包住，如
	() => ({name:'demo'})
	//其实你可以这样理解：圆括号在这里可以避免引擎的对大括号的解析错误：可能是函数体的大括号，也可能是定义
	//对象时的大括号

```


## 箭头函数特点

#### 书写简短

* 箭头函数的写法某些时候比你写函数表达式时候要简短很多，例如：

```javascript
	var arr = [1,2,3,4,5,6];

	//es5方式
	var brr = arr.map(function(item){
			return item * 6;
	});

	//es6方式1
	var crr = arr.map(item => {
		return item * 6;
	});

	//es6方式2
	var drr = arr.map(item => item * 6);

```

由上面的代码我们可以看到，只要合理的书写代码，箭头函数能减小很大一部分书写代码量！


#### 函数体里面未绑定this

* 以前用es5定义函数的时候，如果在函数体里面出现this的话，this总是绑定到特定的对象：1、作为构造函数的时候，this执行新建的那个实例对象；2、以普通函数调用的时候，this指向全局对象，浏览器中指向window；3、在严格模式情况下，this为undefined。如下：

```javascript
	function Demo(){
		this.name = 'demo';
	};

	//普通函数调用
	Demo();
	console.log(window.name)//demo

	//构造函数调用
	var obj = new Demo()
	obj.name //demo
	window.name //undefined

	'use strict';
	Demo();//undefined
```

但某些时候，往往this的绑定的上下文不符合我们的实际需求，我们来看下下面的代码：

```javascript
 function Person(){
 	this.age = 0;
 	setTimeout(function(){
 		//这里我们期望实例化一秒之后，age改变为15
 		//但是因为这点的函数被当成一个普通方法调用了，
 		//所以这里的this绑定到全局，也就是说this.age = 0，但是window.age = 15
 		this.age = 15;
 	},1000);
 };
 var person = new Person();
 setTimeout(function(){
 	 console.log(person.age); //0
 	 console.log(window.age); //15
 },2000);
```

在es5中，我们可以通过以下方法解决这个问题，代码如：

```javascript
	function Person(){
		var _that = this;
		this.age = 0;
		setTimeout(function(){
			//这里我们先把当前对象的引用用变量保存起来从而实现上下文正确绑定
			_that.age = 15;
		},1000);
	};
	var person = new Person();
	setTimeout(function(){
		 console.log(person.age); //15
		 console.log(window.age); //undefined
	},2000);
```

上面代码经过更改之后从而实现了this的正确绑定，但是如果我们用es6中的箭头函数实现的话，不经过处理，this就能正确绑定我们需要的上下文，代码如下：

```javascript
	function Person(){
		this.age = 0;
		setTimeout(()=>this.age=15,1000);
	};
	var person = new Person();
	setTimeout(()=>{
		console.log(person.age);//15
		console.log(window.age);//undefined
	},1100);
```
这是什么原因呢？因为es6箭头函数不会创建自己的this上下文，而是根据当前封闭的作用域内找到this，然后此this就是箭头函数中的this，从而上面的代码就符合我们的工作期望！请记住，this只能是当前封闭作用域下的this而不能是外层作用域的this，如下面的代码：


```javascript
	this.age = 15;
	function Person(){
		var change = ()=>this.age=30;
		change();
	};
	Person();
	console.log(this.age);//15
```

从这里我们知道，当Person当作普通方法调用的时候，内部使用的箭头函数所绑定的this只能在内部，而没有在外部！

当我们用call或apply绑定特定上下文的时候，箭头函数是不会保存this的引用的，看下面的代码：

```javascript
var adder = {
  base: 1,
    
  add: function(a) {
    var f = v =>{
    	console.log(this == true);//true
    	return v + this.base;
    };
    return f(a);
  },

  addThruCall: function(a) {
    var f = v =>{
    	console.log(this == true);//true
    	return v + this.base;
    };
    var b = {
      base: 2
    };
            
    return f.call(b, a);
  }
};

console.log(adder.add(1));         //  2
console.log(adder.addThruCall(1)); //  2 still
```
上面的代码中，我们企图用方法的call方法来改变当前方法的上下文，但都未成功，请记住：只要在一个对象里面用箭头函数调用this，那么this总是指向这个对象，无论使用apply或call与否，这点跟es5是有区别的！

我们再来看一下箭头函数中arguments的绑定情况，在es5中，arguments表示是一个函数参数数组，但是在es6箭头函数中，arguments就不会绑定了，arguments的值是一个查找的过程，看下面的代码：


```javascript
var arguments = 30;
var _test = ()=>arguments;
var a = _test();
console.log(a);//30
function demo(){
	var f = (i)=>arguments[0] + i;
	return f(2);
};
var b = demo(1);
console.log(b);//3
```

由上面的代码可以看出，箭头函数中的arguments并不是指的是函数的参数数组，而是一个某一个具体的变量，如上面的全局变量arguments和demo函数的参数数组（这里因为demo函数不是箭头函数，所以它内部的箭头函数在引用arguments的时候，实际上此时的arguments就是demo函数的参数数组）！

但是我们这里有个问题了，如果我们想在es6中引用一个参数数组，怎么实现呢？此时我们只需要用到es6中的参数扩展就是可实现这个需求，如：

```javascript
var getArgs0 = (...args)=> args[0];
var a = getArgs0('hello','world');
console.log(a);
```