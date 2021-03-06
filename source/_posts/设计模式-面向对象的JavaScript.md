title: 设计模式-面向对象的JavaScript
date: 2018-07-12 18:03:12
categories: [设计模式]
---

# 语言类型
## 静态语言类型
编译时已确定变量类型。优点是可发现类型不匹配错误，对程序规定的数据类型，编译器针对性做优化，提高程序执行速度；缺点是强制为每个变量规定数据类型，增加代码量。

## 动态语言类型
程序运行变量被赋予值后才具有某种类型。优点是编写代码量少，更简洁专注于逻辑表达；缺点是无法保证变量类型。

<!-- more -->

# 鸭子类型（duck typing）
不关心对象的类型，只关心行为。即“如果它走起
路来像鸭子，叫起来也是鸭子，那么它就是鸭子。”利用鸭子类型思想我们就可实现“面向接口编程，而不是面向实现编程”，例如某对象若有push pop方法就可用作栈。

# 多态
同一操作作用于不同的对象，可以产生不同的解释和不同的执行结果。多态思想即把“做什么”和“谁去做以及怎样做”分离，即将“不变的事”和“可能改变的事”分离。

## 对象多态性

    //不变部分，所有动物都会发出叫声
    var makeSound = function(animal){
        animal.sound();
    }

    // 可变部分各自封装
    var Duck = function(){};
    Duck.prototype.sound = function(){
        console.log('嘎嘎');
    }

    var Chicken = function(){};
    Chicken.prototype.sound = function(){
        console.log('咯咯');
    }

    // test
    makeSound(new Duck()); // 嘎嘎
    makeSound(new Chicken()); // 咯咯

# 封装
封装的目的是隐藏信息。包括隐藏数据，隐藏实现细节，设计细节以及隐藏对象的类型。

## 封装数据
使用变量作用域实现封装

    var obj = (function(){
        var _name = 'hollton';
        return {
            getName: function(){
                return _name;
            }
        }
    })();
    console.log(obj.getName()); // hollton
    console.log(obj._name); // undefined

## 封装变化
考虑设计中哪些可能变化，考虑怎样能在不重新设计的情况下进行改变。关键在于封装发生变化的概念。23种设计模式按照意图可划分为创建型模式、结构型模式及行为型模式。
创建型模式即创建一个对象，一种抽象行为，而具体创建什么则是变化的，创建型模式目的就是封装创建对象的变化。结构型模式封装对象之间的组合关系。行为型模式封装对象的行为变化。
通过封装变化，把系统中稳定不变的部分和容易变化的部分隔离分开，后续演变只需替换容易变化部分，保证程序的稳定性和可扩展性。

## 原型模式和基于原型继承的JavaScript对象系统
以类为中心的面向对象编程语言中，对象总是从类中创建。而在原型编程的思想中，类并不是必需，对象未必从类中创建，可通过克隆另一个对象得到。原型模式不单是是一种设计模式，也被称为一种编程泛型。

### 原型编程范型规则
* 所有的数据都是对象
* 对象创建不是通过实例化类，而是找到一个对象作为原型并克隆它
* 对象会记住它的原型
* 如果对象无法响应某请求，会委托给自己的原型

### JavaScript中的原型继承

#### 所有的数据都是对象
JavaScript数据类型：
* 基本类型： Number,String, Boolean,null,undefined
* 对象类型（引用类型）： Object,Function,Array
JavaScript根对象是Object.prototype对象，为一个空对象。JavaScript每个对象实际都是从Object.prototype对象克隆而来，Object.prototype对象为它们的原型。