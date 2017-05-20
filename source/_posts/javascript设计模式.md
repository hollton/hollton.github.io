title: "JavaScript设计模式"
date: 2017-02-25 16:05:40
tags: [JavaScript,设计模式]
categories: [前端]

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
# 第四章 继承
## 类式继承
### 简单类声明
函数声明类，new关键字创建实例。创建构造函数，其名称就是类名，首字母大写，实例属性使用this关键字，方法添加到其prototype对象中。

    /* Class Person */
    function Person (name){
        this.name = name;
    };
    Person.prototype.getName = function(){
        return this.name;
    };
关键字new调用构造函数创建该类实例，即可访问实例属性及方法。

    var hollton = new Person('hollton');
### 原型链
<ol>
	<li>创建一个构造函数，在函数中调用超类的构造函数。<ul><li>使用new时，系统会创建一个空对象，然后调用构造函数，在此过程中这个空对象处于作用域链最前端。而子类调用超类的构造函数时，必须手动完成这项工作。“Person.call(this,name)”使空对象（用this代表），name作参数传入。</li></ul></li>
	<li>设置原型链，指向实例超类<ul><li>访问对象的某个成员时，如果在当前对象中查找不到，javascript会沿着对象的原型链向上逐一访问其原型对象查找，因此将子类的prototype指向超类的一个实例，即可使子类继承超类。</li></ul></li>
	<li>重新设置constructor属性为本构造函数<ul><li>定义构造函数时，默认的prototype对象是Object类型的实例，其constructor属性会被自动设置为该构造函数。但手动将其prototype设置为另一个对象时，其constructor属也变成新对象的constructor值，因此需要重新设置为本构造函数。</li></ul></li>
</ol>

    /* Class Programmer */
    function Programmer(name,programs){
        Person.call(this,name);//当前作用域调用超类的构造函数
        this.programs = programs;//添加新属性
    };
    Programmer.prototype = new Person();//设置原型链
    Programmer.prototype.constructor = Programmer;//重新设置constructor属性
    Programmer.prototype.getPrograms = function(){//添加新方法
        return this.programs;
    };
创建子类实例则不变

	var holltonliu = new Programmer('hollton',['javascript_code']);
### 自定义extend函数
仿照extend关键字自定义函数实现基于给定类结构创建新类。其中添加一个空函数fn，其prototype指向超类的prototype并创建对象实例插入到子类的原型链中。这样是为了避免创建超类的实例（直接使用超类可能：比较大；有副作用；执行大量计算任务）。

	/* Extend function */
	function extend(subClass, superClass) {
		var fn = function(){};
		fn.prototype = superClass.prototype;
		subClass.prototype = new fn();
		subClass.prototype.constructor = subClass;
	}
使用extend函数对上述例子改造：

    function Programmer(name,programs){
        Person.call(this,name);  //存在耦合
        this.programs = programs;
    };
    extend(Programmer,Person);
    Programmer.prototype.getPrograms = function(){
        return this.programs;
    };
存在缺陷：超类（Person）被固化在子类（Programmer）的声明中，为子类添加superPrototype属性解决↓

    /* ---Improved Extend function--- */
    function extend(subClass, superClass) {
        var fn = function(){};
        fn.prototype = superClass.prototype;
        subClass.prototype = new fn();
        subClass.prototype.constructor = subClass;

        subClass.superPrototype = superClass.prototype;
        //超类为Object类或也由他处继承而来时，有可能导致constructor指向不正确，不正确时修复。
        //子类会通过constructor指向调用超类构造函数，因此这个判断修复很重要！
        if(superClass.prototype.constructor == Object.prototype.constructor){
            superClass.prototype.constructor = superClass;
        }
    }
    function Programmer(name,programs){
        Person.superPrototype.constructor.call(this,name);
        this.programs = programs;
    };
    extend(Programmer,Person);
## 原型式继承
### 实现机制
创建一个对象，由于原型链的查找机制，新对象只需重用此对象即可实现继承。

    /* Person Prototype Object */
    var Person = {
        name:'default',
        getName:function(){
            return this.name;
        }
    };
    //customClone创建并返回新空对象，其原型对象指向父级对象
    //当新对象找不到某个属性或方法时会在原型链上查找
    function customClone(superObject) {
        function fn() {};
        fn.prototype = superObject;
        return new fn;  //相当于new fn()
    }
    /* Programmer Prototype Object */
    var Programmer = customClone(Person);
    Programmer.programs = [];
    Programmer.getPrograms = function(){
        return this.programs;
    };
    var hollton = customClone(Programmer);
    hollton.name = 'hollton';
    hollton.programs = ['javascript_code'];
    hollton.getName();
    hollton.getPrograms();
### 继承成员的读写不对等性
在类式继承中，每个实例都有一份自己的副本，而通过原型继承，实例只是以父对象为原型对象的空对象，没为其指定属性时，读取是返指原型链上的同名属性，写入则为其定义一个新属性。这里特别要注意的是，对于引用传递的数据类型如数组，对新对象第一次不赋值写入而做操作如push时会修改其原型链上的数据。

    var hd = customClone(Programmer);
    console.log(Programmer.programs);  //[]
    //不应这么做
    hd.programs.push('javascript_code');
    console.log(Programmer.programs);  //['javascript_code']
    //应该
    hd.programs = [];  //为hd对象添加新属性
    hd.programs.push('javascript_code');
    console.log(Programmer.programs);  //[]
### 工厂方式实现
如果需求继承一个对象而只修改其子对象的的一个属性保留其他，应用上种方法你必须知道对象的全部属性及其默认值并做覆盖操作，显然不合适。

    var CompoundObject = {};
    CompoundObject.name = 'haha';
    CompoundObject.createChildObject = function(){
        return {
            num:10,
            bool:true
        }
    };
    CompoundObject.childObject = CompoundObject.createChildObject();

    var compoundObjectClone = customClone(CompoundObject);
    compoundObjectClone.childObject = CompoundObject.createChildObject();
    compoundObjectClone.childObject.num = 5;  //修改不会影响CompoundObject
## 参元类
包含通用方法的类，用来扩充其他类，称这个通用类为参元类(mixin class)。它们通常不会被实例或直接调用，只是向其他类提供自己的方法。

    /* Mixin class */
    var Mixin = function(){};
    Mixin.prototype = {
        fnName:function(){ ... }
    }
这个方法在不同类中用到时，没必要都去继承它，我们可以自定义个customArgument函数将它添加到需要用到它的类中。

    function customArgument(receiveClass, giveClass){
        for(methodName in giveClass.prototype){
            if(!receiveClass.prototype[methodName]){
                receiveClass.prototype[methodName] = giveClass.prototype[methodName];
            }
        }
    }

    customArgument(Programmer,Mixin);
    var hd. = new Programmer('hollton',['javascript_code']);
    hd.fnName();  //使用参元类方法
# 第五章 单体模式
单体是一个用来划分命名空间并将一批相关方法和属性组织在一起的对象，如果可被实例化，则只能被实例化一次。
## 基本结构

    /* 简单单体（对象字面量） */
    var Singleton = {
        attribute:10,
        method:function(){}
    };
## 拥有私有成员的单体
创建类的私有成员的做法缺点在于较耗内存，因为每个实例都具有方法的一份新副本。由于单体对象只会被实例化一次，因此为其定义私用方法是不必考虑内存方面的问题。
### 下划线标识
### 使用闭包

    MyNamespace.Singleton = (function(){
        //私有
        var privateAtti = false;
        function privateMethod = function(){};
        //公有
        return {
            publicAtti = true;
            publicMethod = function(){};
        };
    })();
这种单体模式又称模块模式，它可以把一批相关的方法和属性组织为模块并起到划分命名空间的作用。
## 惰性实例化
将单体对象实例化推迟到需要使用的时候，称为惰性加载。特别之处是需借助静态方法调用MyNamespace.Singleton.publicMethod -> MyNamespace.Singleton.getInstace().publicMethod。getInstace方法会检查并返回该单体的实例化对象。

    MyNamespace.Singleton = (function(){
        function constructor(){
            var privateAtti = false;
            function privateMethod = function(){};
            return {
                publicAtti = true;
                publicMethod = function(){};
            };
        }
        var uniqueInstance;
        return {
            getInstance:function(){
                if(!uniqueInstance){
                    uniqueInstance = constructor();
                }
                return uniqueInstance;
            }
        }
    })();
# 第六章 方法的链式调用
类的每个方法都return this实现支持方法链式调用的类。
# 第七章 工厂模式
简单工厂使用一个类（通常是一个单体）来生成实例，标准工厂使用子类来决定一个成员变量应该是哪个具体类的实例。
## 简单工厂
简单工厂就像一个单例，它有一个或多个方法来创建或者返回对象。

    /* BicycleShop Class */
    var BicycleShop = function(){};
    BicycleShop.prototype = {
        sellBicycle:function(model){
            var bicycle;
            switch (model){
                case 'speedster':
                    bicycle = new Speedster();
                    break;
                case 'lowrider':
                    bicycle = new Lowrider();
                    break;
                default:
                    bicycle = new Common();
            }
            return bicycle;
        }
    }

    /* Speedster Class */
    var Speedster = function(){};
    Speedster.prototype = {
        assemble:function(){},
        ride:function(){}
    }
    var newBicycleShop = new BicycleShop();
    var newBike = newBicycleShop.sellBicycle('speedster');
当添加一种车型时，不得不BicycleShop代码，更好的方式是在子类中创建车型。
## 标准工厂
工厂是一个将其成员对象的实例化推迟到子类中进行的类。把BicycleShop设计成抽象类，只实现通用方法，让子类各自实现个性如createBicycle方法。

    /* BicycleShop Class (abstract) */
    var BicycleShop = function(){};
    BicycleShop.prototype = {
        sellBicycle:function(model){
            var bicycle = this.createBicycle(model);
            return bicycle;
        },
        createBicycle:function(model){
            throw new Error('createBicycle须由子类实现');
        };
    }

    /* SubBicycleShop Class */
    var SubBicycleShop = function(){};
    SubBicycleShop.prototype = new BicycleShop();
    SubBicycleShop.prototype.createBicycle = function(model){
            var bicycle;
            switch (model){
                case 'speedster':
                    bicycle = new SubSpeedster();
                    break;
                case 'lowrider':
                    bicycle = new SubLowrider();
                    break;
                default:
                    bicycle = new SubCommon();
            }
            return bicycle;
        };
    }
## memoizing技术
memoizing把函数的每次执行结果都存入键值对(数组)，下次直接在键值对中查找值，有则返回该值，没有则执行函数体并再次缓存。
# 第八章 桥接模式
桥接模式最常见和实际的应用场合之一是事件监听器回调函数。
## 示例：事件监听器

    addEvent(ele,'click',getBeerById);
    function getBeerById (e){
        asyncRequest('GET','/getBeer?id='+this.id,function(resp){
            console.log(resp);
        });
    }
作为一个API函数，不应与特定实现混在一起。用桥接模式修改，把抽象隔离开来。

    function getBeerById (id,callBack){
        asyncRequest('GET','/getBeer?id='+id,function(resp){
            callBack(resp);
        });
    }
    addEvent(ele,'click',getBeerByIdBridge);
    function getBeerByIdBridge (e){
        getBeerById(this.id,function(beer){
            console.log(beer);
        });
    }
如果一个桥接函数被用于连接两个函数，而其中某个函数根本不会在桥接函数外被调用，那么这个桥接函数就是不必要的。
# 第九章 组合模式
