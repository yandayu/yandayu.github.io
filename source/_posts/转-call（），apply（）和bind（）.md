---
title: '[转]call（），apply（）和bind（）'
categories:
- 前端
tags:
- javascript
date: 2018-11-14 17:07:44
---


今天对于call方法和apply方法有些懵，所以去看了些别人的总结，感觉有了点概念，把一些大佬写的东西中自己感觉易懂的解释和经典的案例记录一下。

## 定义

call方法: 
语法：call([thisObj[,arg1[, arg2[,   [,.argN]]]]]) 
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 
说明： 
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。（ 第一个借用别人的函数，第二个借用别人的上下文环境。） 如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。
用括号的第一个参数来代替this的指向
call（） 就是用来让括号里的对象 来集成括号外的函数的属性！可以称之为继承！

apply方法： 
语法：apply([thisObj[,argArray]]) 
定义：应用某一对象的一个方法，用另一个对象替换当前对象。 
说明： 
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数

作用相同，只是书写格式不同

## call方法案例
```javascript
    function add(a,b)
    {
        alert(a+b);
    }
    function sub(a,b)
    {
        alert(a-b);
    }

    add.call(sub,3,1); 
```
个人理解call和apply的作用就是切换函数的对象上下文。

“这个例子中的意思就是用 add 来替换 sub“，应该是将add执行的上下文由window切换为sub，即this指向是从window变为sub，仅此而已，并非add替换sub。这个例子很难说明什么。
其实就是call方法前面的东西(add)都交给后面(sub)了，就像兄弟，接过这把枪。

```javascript
    add.call(sub,3,1) // sub 已经接过了add方法

    function Animal(){    
    this.name = "Animal";    
    this.showName = function(){    
    alert(this.name);    
     }    
    }     
    function Cat(){    
     this.name = "Cat";    
    }      
    var animal = new Animal();    
    var cat = new Cat(); 

     
//通过call或apply方法，将原本属于Animal对象的showName()方法交给对象cat来使用了。    
//输入结果为"Cat"    
//animal.showName.call(cat,",");    
//animal.showName.apply(cat,[]);  
```
call 的意思是把 animal 的方法放到cat上执行，原来cat是没有showName() 方法，现在是把animal 的showName()方法放到 cat上来执行，所以this.name 应该是 Cat。

'call 的意思是把 animal 的方法放到cat上执行'这个应该是`animal.showName`调用时候，将animal中的this对象转变为cat，`alert(this.name)`这时候的this是cat，因此`this.name==cat.name`,所以输出是Cat。

## 补充

关于javascript中call和apply函数的应用 
我们经常在javascipt中的面向对象应用中遇到call和apply函数；有时会被搞糊涂。其实它们可以改变函数或对象中的this保留字的值；this保留字的默认值就是这个类本身。举例说明： 
复制代码 代码如下:
```html
    <!DOCTYPE html> 
    <html xmlns="http://www.w3.org/1999/xhtml"> 
    <head> 
    <meta http-equiv="Content-Type" content="text/html； charset=gb2312" /> 
    <script language="javascript"> 
    test = { 
    value: 'default'，exec: function() { 
    alert(this.value)； 
    } 
    } 
    function hhh(obj) { 
    test.exec()；test.exec.apply(obj)； 
    } 
    </script> 
    </head> 
    <body> 
    <input type="button" onclick="hhh(this)；" value="test" /> 
    </body> 
    </html> 
```
    运行以上的页面就很快明白了. 
    call和apply函数可以处理匿名函数 
    关于类的初始化应用如下： 

 ``` javascript  
    Person = function() { 
    this.Init.apply(this, arguments); 
    }; 
    Person.prototype = { 
    first: null, 
    last: null, 
    Init: function(first, last) { 
    this.first = first; 
    this.last = last; 
    }, 
    fullName: function() { 
    return this.first + ' ' + this.last; 
    }, 
    fullNameReversed: function() { 
    return this.last + ', ' + this.first; 
    } 
    }; 
    var s = new Person2('creese', 'yang'); 
    alert(s.fullName()); 
    alert(s.fullNameReversed()); 
```
call和apply函数可以赋值函数内容（带匿名参数；但不触发） 
关于函数绑定事件应用如下： 

```javascript
    Function.prototype.BindForEvent = function() { 
    var __m = this， object = arguments[0]， args = new Array()； 
    for(var i = 1； i < arguments.length； i++){ 
    args.push(arguments[i])； 
    } 
    return function(event) { 
    return __m.apply(object， [( event || window.event)].concat(args))； 
    } 
    } 
```
call和apply函数关于函数绑定参数应用如下： 

```javascript
    Function.prototype.Bind = function() { 
    var __m = this， object = arguments[0]， args = new Array()； 
    for(var i = 1； i < arguments.length； i++){ 
    args.push(arguments[i])； 
    } 
    return function() { 
    return __m.apply(object， args)； 
    } 
    } 
```
  call和apply函数功能是一样的；就是参数格式不同；fun.call(obj， arguments)；apply的arguments是数组形式；call则是单数形式。

## bind()方法

  bind与前两个不同，前两个是改变this的指向，而bind是绑定this的指向，返回的是一个this指向为你所绑定的一个新函数，而原本的函数并没有改变。并且，call和apply改变this指向时会自动调用此函数，但bind不会，需要手动去调用并传参。如果在绑定this指向时传参，那参数的值就不能再改变。

**语法**

`bind（this指向）`




