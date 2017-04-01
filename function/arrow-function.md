### 简介

* **什么是箭头函数（arrow-function）?**在es6中，箭头函数就是你在es5中所写函数形式的一种简短形式！简而言之，跟你以前写过的函数主要有两个区别：一、书写方式不同（外形上的不同）；二、未绑定this、arguments等。

* 箭头函数在语法上主要有以下几种方式：

```code
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