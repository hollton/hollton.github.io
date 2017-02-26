title: "javascript设计模式"
date: 2017-02-25 16:05:40
tags: [javascript,设计模式]

---
# 第一章 富有表现力的javascript
## javascript的灵活性
### 过程式程序设计模式
	function startRun(){ ... };
	function stopRun(){ ... };

特点：简单，但无法创建可以保存状态且具有仅对其内部状态进行操作的方法的对象。
### 定义类，并把方法赋给该类的prototype属性
	var Run = function(){ ... };
	Run.prototype = {
		start:function(){ ... },
		stop:function(){ ... }
	};
	/* usage */
	var myRun = new Run();
	myRun.start();
### 自定义为类添加新方法
	/* 给函数添加可自定义方法的方法 */
	Function.prototype.customMethod = function(name,fn){
	    this.prototype[name] = fn;
	    //以下代码可使customMethod被链式调用
	    //return this;
	};
	var Run = function(){ ... }
	Run.customMethod('start',fuction(){
	    ...
	});
## 弱类型语言
### 原始数据类型
布尔（Boolean），数值（Number不区分整数、浮点数），字符串（String），空（Null），未定义（Undefined）：按值传送
### 复合数据类型
对象（Object：无序键值对；数组特殊对象：为有序集合）：按引用传送
# 第二章 接口
## 定义
接口提供了一种用以说明一个对象应该具有哪些方法的手段。我的理解是接口声明了一个方法所需的参数及返回的数据，并在类中去实现它。
## js模仿接口
### 注释描述
在注释中使用interface,implements关键字。实现简单但仅限制于文档规范阶段，并没做实现检查，也不会抛出错误提示。

	/*
		interface Composite {
			function add(item);
		}
	*/
	var CompositeForm = function(id,method,action){  //implements Composite
	};
	//implement the Composite interface
	CompositeForm.prototype.add = function(item){ ... }
### 属性检查
即通过检查属性判断是否实现此接口，没有则抛出错误，但并未对接口的真正实现做检查。
### 鸭式辨型
不关注是否声明支持哪些方法，而只要具有这些接口的方法就行。双重遍历检测每个接口是否实现了某种方法.
## interface类
接口在运用设计模式实现复杂系统时能体现其价值，可降低对象间的耦合度，但对于小型项目，好处并不明显且徒增复杂度。
# 第三章 封装和信息隐藏
## 封装是面向对象设计的基石
将方法或属性声明为私用，可以让对象的实现细节对其他对象保密以降低对象间耦合度，且可以保持数据的完整性并对其修改方式加以约束。js中可使用闭包创建只允许从对象内部访问的属性和方法，以达到封装数据的效果。
## 创建对象的基本模式
### 门户大开型
传统方式，用函数做其构造器，属性和方法公开，用this关键字创建属性。

	var book = function(title){
		this.setTitle(title);
	};
	book.prototype = {
		getTitle:function(){
			return this.title;
		},
		setTitle:function(title){
			this.title = title;
		},
		display:function(){ ... }
	};
### 命名规范（_）
属性和方法前添加下划线（_）以示其私有性。
### 作用域、嵌套函数、闭包
闭包函数能够访问另一个函数的内部变量，函数作用域内return一个嵌套函数即构成闭包。
之所以会产生闭包是因为js的作用域是词法性的，即函数运行在定义它们的作用域中，而不是调用它们的作用域。

	var book = function(newTitle){
		//私有属性
		var title;
		//特权方法（privileged method，公用却能访问私有属性）
		this.getTitle = function(){
			return title;
		};
		this.setTitle = function(newTitle){
			title = newTitle;
		};
		//构造函数代码
		this.setTitle(newTitle);
	};
	//公用方法
	book.prototype.display = function(){ ... };
前两种方式的方法都创建在原型对象中，无论生成多少对象实例，方法只在内存中存一份。而采用闭包则每生成一个对象实例都为每一个私有方法和特权方法生成新副本，更耗内存。