---
title: '[原]Es6'
categories:
- 前端
tags:
- es6
date: 2019-01-02 21:23:51
---

**let声明变量**

 1. 声明不会提升

 2. 不能重复声明（当前作用域下不能有相同变量名），会报错

 3. 变量会捆绑在当前的语句块中，也就是说只在当前块级作用域下生效

**const声明常量**

 1. 声明不会提升

 2. 不能重复声明，会报错

 3. 在声明常量时，必须要赋值，否则会报错

 4. 一经赋值不可修改

 **函数参数的默认值**

 `function(color = 'red'){}`
 red为color的默认值

**箭头函数**

`var max = function(){}`

`var max = () => {}`    这行代码等价与上面的代码
当代码简洁时，可以把大括号去掉简写为一行

eg：`var max = (x,y) => x>y?x:y;`
=> 后面的代码为return的值，但不能写return

如果函数的参数不止一个或没有，要用小括号包着，如果只有一个参数，可以省略括号。

箭头函数中没有this，如果使用this是指向window的

**剩余操作符 ...**

我们可以在形参名前加上...来让它接受剩余的参数，当不确定实参个数时，这是个很好的选择。

`function sum(a,b,...others){}
sun(1,2,3,4,5,6,7)`

a=1,b=2,others就等于一个数组，里面包含所有剩下的实参，也就是[3,4,5,6,7]

**展开操作符...**

我们可以在数组名前加上展开操作符，那么他就会显示数组中的各个元素

eg:  `let arr1 = [1,2,3]     console.log(...arr1)`
控制台会打印1,2,3

我们也可以使用这个操作符来复制/合并数组
复制：let arr2 = [...arr1]  

合并：let arr2 = [...arr1,4,5] 

**模板字面量**

使用模板字面量来定义字符串，可以使用${}在字符串中插入内容，在括号中可写变量或表达式
并且在模板字面量中换行，会保留格式，换行和tab键都会放在字符串中

```javascript
var str = `<tr><td>${ name }</td></tr>`
```
**对象的属性简写**

当属性值和属性名相同时，可简写为一个变量名

```javascript
var name = '李四'
var obj = { 
name
//这里的name值为李四
}

```
**数组解构**

```javascript
let colors=  [ 'red','green','blue'];
let [firstC,secC,thirdC] = colors;
// 上面的变量的值按顺序依次为数组中的颜色
```
当为变量重新赋值时，只需将let 去掉即可。
为变量设置默认值，与函数形参默认值方法相同。

嵌套数组解构：
```javascript
let colors=  [ 'red','green','blue',['violet','white']];
let [firstC,secC,thirdC,[,lastC]] = colors;

```
**对象解构**

```javascript
let obj1= {name:'lisi',age:20};
let {name,age} = obj1;

```
与数组解构不同的是,上面这种方法只能当变量名和对象的属性名相同使可以使用，如果要为非同名的变量赋值，要写为以下的方式

```
let {name:myName,age:myAge} = obj1;
```
前面的是对象中的属性名，后面的为我们设置的变量名，相当于为变量指明要从哪里找值。
默认值：与函数形参的默认值的传值方式相同
为变量重新赋值：因为JavaScript不允许赋值操作符左边出现花括号，所以我们要将整个赋值语句用小括号包裹起来，将赋值语句变为一个表达式。

```
({name:myName,age:myAge} = obj1;)
```

**模块**

*导出*：export {名称} /export 函数/变量

*导出时重命名*：export{旧名字 as 新名字}

*导出默认值*：export default 名称    
            export { 名称 as default }
            export default 名称

*导入*：import {名称} from 

*导入所有*： import *as 名称 from 文件路径     用这个名称来使用，是一个对象，所有模块导出的东西都在里面

*导入时重命名*：import {旧名字 as 新名字}  from 文件路径

*导入默认值*：import  名称   from 文件路径             默认值会被自动绑定到此名称上
一个模块的默认值只能有一个。

*如果在一个js文件中导入模块，但并不使用它，可以再导出，起桥梁作用*

`export {名称} from 文件路径`

*如果一个文件中没有导入或导出，只是修改了一些全局方法*，那么我们在另一个页面导入是，应该：import 文件路径