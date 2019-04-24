---
title: '[转]Evevt Loop 事件循环'
categories:
- 前端
tags:
- javascript
date: 2019-01-02 20:26:40
---
**今天在看es6的promise的时候，看了一个很有意思的面试题，牵扯到了setTimeout的执行顺序，总结一下**
**贴一下面试题的链接** Excuse me？这个前端面试在搞事！ - Liril的文章 - 知乎https://zhuanlan.zhihu.com/p/25407758

![事件循环](https://raw.githubusercontent.com/yandayu/yandayu.github.io/img/images/Event%20Loop.png)

同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
当指定的事情完成时，Event Table会将这个函数移入Event Queue。
主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
上述过程会不断重复，也就是常说的Event Loop(事件循环)。

**setTimeout**

如果以0毫秒的超时时间来调用setTimeout(),那么指定的函数不会立即执行，相反会把它放到队列中去，等到前面处于等待状态的事件处理程序全部执行完成后，再“立即”调用它。（setTimeout和setInterval经常有不守时的时候）。
setTimeout并不是真正的异步操作，只是把想执行的代码放到UI队列中，在未来执行

 **Es6中的Promise，里面的函数是直接执行的，比setTimeout执行的早**