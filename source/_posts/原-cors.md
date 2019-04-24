---
title: '[原]cors'
categories:
- 前端
tags:
- cors
date: 2019-04-12 21:23:51
---

**跨域**

传统解决跨域方案有
1. jsonp,它利用了`script`标签的src属性可以跨域的特性但它只能发送get请求，局限性很大。
2. 代理服务器，浏览器端配置代理服务器，但只能用在开发阶段。
3. 后台配置允许跨域，但同源策略的安全性就没有了。

**cors是一种跨域的解决方案，它根据HTTP请求头告诉浏览器被准许访问不同源服务器上的资源，需要前后台配合**

cors有两种请求：

 1. 简单请求

    当请求方式为 GET,POST,HEAD或content-type的值为 text/plain，multipart/form-data,application/x-www-form-urlencoded满足任一条即会发送简单请求。

    请求头中会发送`origin`字段表明自己的来源，后台返回的响应信息中有`Access-Control-Allow-Origin`字段表明服务器允许访问的主源。
 2. 预检请求

    当请求方式为上述以外的五种或content-type的值不为以上三种即会发送预检请求。

    浏览器首先会发送OPTIONS的预检请求，请求头中会发送`origin`，`Access-Request-Method`(值为前台请求方式)，`Access-Control-Request-Headers`(实际请求头)，服务器接收到预检请求后返回服务器所能接收的`origin`，请求方式和请求头，当服务器同意后，客户端发送真正的请求，服务器响应。