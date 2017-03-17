title: JavaScript语言精粹
date: 2017-03-14 12:12:23

---
# 第一章 精华
# 第二章 语法
## 铁路图
<ul>
	<li>从左边开始，沿着轨道到右边界</li>
    <li>圆框中是字面量，方框中是规则或描述</li>
    <li>任何沿着轨道能走通的序列都是合法的，反之是非法的</li>
    <li>末端只有一个竖条的铁路图，表示允许在任意一对符号中插入空白。而末端有两个竖条的则不允许。</li>
</ul>
## 数字
指数表示法：字面量值等于e之前的数字与10的e之后数字的次方相乘，即 100 等于1e2.
NaN：数值，表示不能产生正常结果的运算结果，不等于任何值NaN!=NaN，通过isNaN(number)检测。
## 语句
for in 会枚举对象的所有属性名（或键名），通过hasOwnProperty()检测某属性是该对象的成员，还是来自原型链。

    for (myvar in obj){
        if(obj.hasOwnProperty(myvar)){
            ...
        }
    }
break使程序退出循环或switch语句，如果指定标签则退出带有该标签的程序段（可用于退出嵌套循环，continue同）

    hollton : for(var i=0;i<3;i++){
        for(var j=0;j<3;j++){
            if(i===1&&j===1){
                break hollton;
            }else{
                console.log("i:"+i+"--j:"+j);
            }
        }
    }
如果没指定标签，break只退出内部循环，指定标签后，则退出标签指定的代码段，即整个循环。
# 第三章 对象
## 引用
对象通过引用传递，永远不会被复制。

    var x = {};
    var y = x;
    y.name = 'hah';
    console.log(x.name);  //'hah',因为x,y指向同一个对象的引用

    var a = {},b = {};  //a,b引用一个不同的空对象
    var a = b = {};  //a,b引用同一个的空对象
## 原型
每个对象都连接到一个原型对象，并可从中继承属性。原型连接只在检索值的时候才被用到，获取某个对象的某属性值时，若该对象没此属性名，则js从原型对象中获取，若原型对象中也没有，再从原型链上获取，直到Object.prototype，仍不存在返回undefined。这个过程称为委托。
# 第四章 函数
## 调用
调用一个函数会暂停当前函数的执行，传递控制权和参数给新函数。函数接收两个附加的参数：this和arguments。this的取值取决于调用的模式。
JavaScript共有4种调用模式：方法调用模式、函数调用模式、构造器调用模式、apply调用模式。
## 方法调用模式
当函数被保存为对象的属性时，称之为方法。当方法被调用时，this被绑定到该对象，通过this可获取到所属对象的上下文的方法称为公共方法。
## 函数调用模式
当函数不是对象的属性时，则作为函数调用，此时this被绑定到全局对象。导致内部函数不能通过this访问外部函数的属性及方法，但可通过在外部函数定义变量（如_this,that）并赋值为this，可使内部函数访问。
## 构造器调用模式
若函数通过new调用，则会创建一个新对象，该新对象连接到该函数的prototype成员，同时this被绑定到该新对象上，即this指向该构造器实例。
## Apply调用模式
Fn.apply(obj,args)方法接收两个参数：
<ol>
    <li>obj是要绑定给this的值，即obj将代替Fn里的this对象</li>
    <li>args是要传递给调用函数的参数数组，即args-->arguments</li>
</ol>
call同apply，只有参数列表不一样 ：Fn.call(obj,arg1,arg2,...)
## 参数
函数被调用时，可以通过arguments访问传递的参数列表，包括多余形参。arguments是类似数组的对象，拥有length属性，但没有任何数组方法。
## 返回
函数总会返回值，若没指定则返回undefined。若通过new调用函数且返回值不是对象，则会返回this，即该新对象。
## 闭包
函数作用域使得内部函数可访问其外部函数的参数和变量（除this和arguments）。

    //创建hollton构造函数，构造出带有getName方法和name私有属性的对象
    var hollton = function(name){
        return {
            getName:function(){
                return name;
            };
        }
    }
    var hd = hollton('holl');
    console.log(hd.getName());
当调用hollton时，它返回包含getName方法的新对象，即使hollton已经返回了但getName仍有特权访问hollton对象的name属性，称为闭包。
## 模块
可使用函数和闭包构造函数，模块是一个提供接口而隐藏状态及实现的函数或对象。
模块的一般形式：定义了私有变量和特权函数的函数。利用闭包创建可以访问私有变量和函数的特权函数，并返回这个特权函数。
## 柯里化
柯里化允许把函数与传递来的参数相结合，并产生一个新的函数。

    function curry(fn){
        var args = Array.prototype.slice.call(arguments, 1);
        return function(){
            var innerArgs = Array.prototype.slice.call(arguments);
            var finalArgs = args.concat(innerArgs);
            return fn.apply(null, finalArgs);
        };
    }
## 记忆
函数可以将先前的结果记录在某个对象里，从而避免无谓的重复计算。