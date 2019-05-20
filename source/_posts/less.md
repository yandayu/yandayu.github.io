---
title: 'less'
categories:
- 前端
tags:
- less
date: 2019-01-16 21:25:59
---

**我们在项目中使用less时，可以分开创建各个类型，创建一个专门保存变量的less 文件，一个混入方法的文件等，有利于以后的维护**

**声明编码类型防止乱码**
```less
@charset "utf-8";    
```
**变量**
```less
/*必须@前缀,;结尾,:为等于。变量名不能以数字开头，不能包含特殊字符，区分大小写 */
@textColor:#333;
a{
    color: @textColor;
}
/* 也可以用来声明一个类名，但变量与其他字符拼接时，要加{}来表明哪个是变量 */
@className:box;
.@{className}{
    color:@textColor;
}
```
**mixin混入**
```less
/* 类混合，这种方式类也会被编译进css中，但是我们不会使用，所以使用函数的形式 */
.h50{
    height: 50%;
}
.f_r{
    float: right;
}
.h50-f_r{
    .h50;
    .f_r;
}
/* 定义函数，函数不会被编译进css中 */
.w50(){
    width: 50%;
}
```
```less
/* 传参的函数，如果没有默认值，使用时必须要传参;不传参时不写括号
    默认值写法与变量写法相同 */

.f(@direction:left){
    float: @direction;
}
.w50-f{
    .w50;
    .f(right);
}
```
**嵌套**
```less
.header{
    display: block;
    img{
        display: none;
    }
    /* 需要连接时使用：& */
    &:hover{
        img{
            display: block;
        }
    }
}
/* 会解析为以下形式*/
   .header{
       display:block
    }
   .header img{
       display:none
    }
   .header:hover img{
       display:block
    } 
```
**使用@import 导入文件**

```less
@import "./main.less";
a{
  background: @bgc;
}
```

**转义：~"",放在此引号中的任何东西都会被转义，原样输出**

```less
@minw: ~"(min-width:768px)";
.box{
    @media @minw{
        width: 400px;
    }
}
```
**并且指令会冒泡，为以下形式**
```less
@media (min-width:768px) {
  .box {
    width: 400px;
  }
} 
```
**命名空间**
```less
#group1(){
    .btn{
        -webkit-appearance: none;
        width: 40px;
        background-color: #ccc;
        border: 1px solid #fff;
    }
}
/* 使用时 */
.box button{
    #group1 >.btn
}
```
**循环**
mixin可以调用本身，结合Guard和模式匹配，可以创建各种循环/迭代的结构。
下面是官网给的例子，when是当条件满足时执行函数
```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}
```
**在做移动端rem适配时使用了此方法，便于维护，并且用到了几个内置函数**
less的变量可以定义数组：`@deviceList:750px,640px,340px;`
拿数组的长度length()：`@len:length(@deviceList)`
拿数组中的数据extract()
```less
@index:1;
extract(@deviceList,@index)
```