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

## 封装实现
