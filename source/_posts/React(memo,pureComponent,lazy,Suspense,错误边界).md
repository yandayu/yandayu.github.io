---
title: 'React(memo,pureComponent,lazy,Suspense,错误边界).'
categories:
- 前端
tags:
- react
date: 2019-07-02 20:26:40
---

#### 1. React.memo() 
用来控制何时重新渲染组件，与pureComponent类似，但它是一个高阶函数，是一个函数组件而非类。

使用方法： `React.memo(()=>{}，true/false)；`第二个参数用来控制是否刷新。

何时使用：假设一个类或函数中有一个子组件与它自己的ReactNode，当ReactNode中引用的变量发生变化时，子组件也会重新渲染，为了避免这种无故的重新渲染可以使用此函数或pureComponent，它们会在组件所接收到的props数据发生改变时才会重新渲染。

 #### 2. pureComponent 依靠class使用

使用方法： `class Test extends PureComponent`

**注意**

只有传入的props的第一级发生改变时才会触发重新渲染，也就是说我们在pureComponent创建的组件上传入一个对象，当对象中的一项发生改变时，组件不会重新渲染。记好这点避免出现视图不渲染的组件

`<Test cb={()=>{}}/>` 以此为例，向组件传入一个回调，组件每次都会刷新，因为传入的内联函数每次都是新的

#### 3. React.lazy() 延迟加载方法（不适合服务器端渲染）

之前引入文件的方式：`import Mine from './Mine';`
使用React.lazy()方法：`const Mine = React.lazy(()=>import('./Mine'));` 这种动态引入方法会使组件在渲染时再去加载此文件。这个函数中包含了一个动态的import,动态的引入支持Promise的API规范。

但是在这样使用Mine组件的时候我们发现页面报错了，因为这个页面还没有被加载完成，React不知道此时Mine组件位置应该去显示什么，就像我们平时请求一个东西还没有返回结果时，我们通常会显示一个loading

#### 4. Suspense 正是这个‘loading’,加载的过程中显示Suspense所提供的内容，加载完成后显示Mine组件

使用方法：
```javascript
 import Suspense from 'React;
 <Suspense fallback={<div>loading...</div>}>
    <Mine/>
 </Suspense>
```

#### 5. 错误边界

当模块加载失败，会触发一个错误，可以通过错误边界来处理这些情况，进行错误的捕获和保护

创建错误边界：可以组件中定义static getDerivedStateFromError(error)或者componentDidCatch(error,info)