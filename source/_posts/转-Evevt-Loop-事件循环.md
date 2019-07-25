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


### 补充

事件循环时通过任务队列的机制来协调的，一个事件循环中可以有一个或多个任务队列，
一个任务队列就是一系列有序任务的集合，每个任务都有任务源，源自同一个任务源的任务必须放到同一个任务队列，不同源则被放到不同队列，任务又分为 **宏任务**和**微任务**

宏任务主要包含：script整体代码，setTimeout,setInterval,I/O,ui交互事件，postMessage,MessageChannel,setImmediate(node环境)。

微任务主要包含：Promise.then,MutaionObserver,process.nextTick(node);

**setTimeout/Promise等API便是任务源，进入任务队列的时它们所执行的具体任务，不同源进入不同队列，其中setTimeout与setInterval是同源的。**

js中所有的任务可以看作存放在两个队列中 —- 执行队列和事件队列

执行队列中是所有同步代码的任务，事件队列是所有异步代码的宏任务

js会优先执行完所有同步代码，遇到对应的异步代码放在对应的队列，（宏任务放到事件队列，微任务放到执行对列之后，事件队列之前），之后执行微任务，最后是事件队列。

执行队列 --> 微任务 --> 事件队列（宏任务）
