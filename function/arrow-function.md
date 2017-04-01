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