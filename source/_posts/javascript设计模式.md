title: "javascript设计模式"
date: 2017-02-25 16:05:40
tags: [javascript,设计模式]

---
##第一章 富有表现力的javascript
###1.1 javascript的灵活性
#####1.1.1 过程式程序设计模式
    function startRun(){ ... };
	function stopRun(){ ... };
特点：简单，但无法创建可以保存状态且具有仅对其内部状态进行操作的方法的对象。
#####1.1.2 定义类，并把方法赋给该类的prototype属性
    var Run = function(){ ... };
	Run,prototype = {
		start:function(){ ... },
		stop:function(){ ... }
	};
	/* usage */
	var myRun = new Run();
	myRun.start();
#####1.1.3 自定义为类添加新方法
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
###1.2 弱类型语言
#####1.2.1 原始数据类型
布尔（Boolean），数值（Number不区分整数、浮点数），字符串（String），空（Null），未定义（Undefined）：按值传送
#####1.2.2 复合数据类型
对象（Object：无序键值对；数组特殊对象：为有序集合）：按引用传送
##第二章 接口
###2.1 
接口提供了一种用以说明一个对象应该具有哪些方法的手段
