---
title: '[原]typescript'
categories:
- 前端
tags:
- typescript
date: 2019-01-23 13:04:28
---

 1. **变量声明**

```javascript
// 变量声明,约束形参tel的类型
function phone(tel:number) {
    console.log(tel)   
}
phone(1234)
```

 2. **接口**

```javascript
// 定义接口。用来定义一些规则，使用此规则就要严格遵守
interface obj {  //obj相当于这些规则的名字
  readonly name:string;  // 表示此属性是只读的
    age?:number;  // 在属性后加？表示不是必需的，可不传
}
// stu函数的参数student用了这个规则，所以student中的属性必须要有一个名为name并且值的类型为字符串。属性的顺序不加规定
function stu(student: obj){
    let newStudent = {
        name:'lisa',
        age:22,
        gender:'女'
    }
    if (student.name) newStudent.name=student.name;
    return newStudent;
}
let newStu = stu({name:'pula'})
console.log(newStu)
```

 3. **数组**
```javascript
let arr1: string[]=['1','2']  // 定义一个字符串类型的数组。数组类型写在[]前
let arr2: ReadonlyArray<string> = ['3','4']  //定义一个只读的字符串型的数组
// 就算是把arr1赋给arr2也不行，只能重改类型
arr1 = arr2 as string[];
// 下面这种方式也是不行的，因为arr2不允许改变
// arr2 = arr2 as string[];

// 如果我们定义了一个规则,这里的color属性是可选的
interface colors{
    color?: string; 
    [property: string]: any;    
}
function create(crec:colors) {
    console.log(crec)
}
    create({ colour: 'red' }) 
//  在这里我们传了一个属性为colour的属性，但它仍然会报错，
//  因为对象字面量会经过额外属性检查，当将它们赋值给变量或作为参数传递的时候。 如果一个对象字面量存在规则中不包含的属性时就会报错
//  解决方法：在接口中定义 [property:string]: any ;  表示crec可以有任意数量的其他属性。
//  官方文档说另一种解决方法是把对象使用变量保存一下再传入函数中，但是试了试还是报错，原因未知，所以先使用上一个方法。
```

 4. **索引签名**
```javascript
// 声明索引类型及返回的类型,索引签名只能是string或number。当索引签名为数字时，系统会自动转为字符串来索引
// 所以数字索引的返回值必须是字符串索引返回类型的子类型(通过extends来创建的派生类)

interface NumberDictionary {
    [index: number]: string;
    // [index: string]: number; 这样时错误的
}
interface StringDictionary {
    readonly [index:string]:number    //这样就不能通过索引来改变值
}
var arr3: NumberDictionary;
arr3 = ["aa", "bb", "cc"];  // 同时数组中的值只能为字符串
var arr4: StringDictionary
arr4 = { 1: 44, 2: 55 };
console.log(arr3[1]);
console.log(arr4[1]);  //所以这里即使定义的是字符串却依然可以使用数字来索引,得到的值是一样的
##类##

class Person {
    public name: string;  // public修饰符，成员可以在任何地方访问，默认问public
    private age: number;  // 添加了private只允许在这个类中访问
    constructor(tname:string,tage:number){
        this.name = tname;
        this.age = tage;
    }
    greeting(){
        console.log(`Hi,I am ${this.name} and I am ${this.age} years old`)
    }
}
// protected也允许在派生类访问，还有我们之前用的readonly修饰符，表明是只读属性，必须在声明或构造函数里被初始化类似于const
```
5. **继承**
```javascript
// 继承
class Student extends Person {
    constructor (name,age){
        super(name,age)   // 在构造函数里访问this属性之前必须使用super方法
    }
    greeting (){
        console.log('子类重写的方法')  // 子类重写了greeting方法
        super.greeting()   // 必须调用super才能调用基类的greeting函数
    }
}
var per = new Person('lisa',22);
var stud = new Student('pupu',25);
per.greeting()  //打印： Hi,I am lisa and I am 22 years old
stud.greeting() //第一次打印：子类重写的方法；第二次： Hi,I am pupu and I am 25 years old
```