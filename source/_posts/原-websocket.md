---
title: '[原]WebSocket'
categories:
- 前端
tags:
- WebSocket
date: 2019-04-17 21:23:51
---

**定义**

WebSocket是h5提供的一种新的协议。它可以在单个TCP链接上进行双全工通信，是一个持久化的协议，并且它允许服务器主动连接浏览器，使服务器与浏览器之间的通信变得更加方便

**原理**

传统的HTTP协议中，只能浏览器往服务器端发送请求，服务器响应，但不能获取到最新的消息，于是使用ajax轮询，去一直像服务器发送请求，但这样大大浪费了资源并且服务器也需要很快的处理速度，http long poll与ajax轮询的原理也差不多，都不是最好的解决方式。而WebSocket只需要一次连接，浏览器告诉服务器是WebSocket链接，有消息后再告知我。

**发送websocket请求**

```javascript
let ws = new WebSocket(url[,options]);
ws.onopen = finction(){}  
//连接成功时触发
ws.onmessage = function(){}  
//收到服务器响应的消息时触发
ws.onerror = function(){}  
//错误时触发
ws.onclose = function(){} 
 //关闭时触发
 ws.send()
 //发送消息
 ws.close()
 //关闭连接
 ws.readyState  
 //连接状态，与ajax的类似

```
**WebSocket连接中浏览器请求头会多以下字段**

`Upgrade: websocket`

`Connection: Upgrade`

*以上告诉服务器是websocket连接*

`Sec-WebSocket-Key: 浏览器随机生成的base64编码16位字符串`

`Sec-WebSocket-Protocol: 表示浏览器要使用的协议`

`Sec-WebSocket-Version: 协议版本号`

服务器响应：

`Upgrade: websocket`

`Connection: Upgrade`

`Sec-WebSocket-Accept: 经过服务器确认并加密后的Sec-WebSocket-Key`

`Sec-WebSocket-Protocol: 最终使用的协议`