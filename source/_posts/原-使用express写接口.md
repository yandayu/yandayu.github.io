---
title: '[原]使用express写接口'
categories:
- 前端
tags:
- node
date: 2019-01-09 16:33:16
---

## express是一个基于node.js的web开发框架

 首先要安装express插件并引入到js文件中，express是一个函数，所以要调用，调用后会返回一个实例（appplication），
使用实例的一些方法来完成我们的项目。

 **express有四种方法，前三种为中间件方法**：
     
 1. `express.static(root[,option])`

```javascript
// 托管静态文件
app.use(express.static('public'));
```

app为express的实例，因为托管静态文件是中间件方法，所以使用了它的use方法

2. `express.json`

 这个方法是用来解析请求体的内容（解析application/json编码类型的数据），因为请求体中默认为空，当解析时才会被填充
 
 3. `express.urlencoded([options])`

 与上个方法一样，只是用来解析application/x-www-form-urlencoded"编码类型的数据。这两中方法都要在版本v4.16.0 以上才能使用。如果版本低，可以使用body-parser插件来解析
 
 4. `express.Router([options])`

 创建一个路由器对象，当一个js文件中有多种类型的接口时，我们可以把一种类型的拿出来，放在另一个js文件中，引入需要的插件，使用router实例代替app实例调用各种方法，最后导出整个页面。
 
**response方法（只列出了用到的方法，具体官网查询）**
   
`response.send()`  向浏览器发送消息

`response.end() `   结束响应的进程

`response.status()`  设置响应的HTTP状态

`response.header()`  设置响应头

**request**

`request.body`    请求体

**application**

`app.METHOD(path，callback)`   
METHOD可以为get,post,put等，使用指定的回调函数将HTTP请求路由到指定的路径。

实际方法：`app.get(path，callback)`
`app.listen(port[,callback])`      监听指定端口
`app.use([path,] callback [, callback...])`   安装指定的中间件在指定路径上执行的一个或多个函数当所请求路径的匹配时执行中间件函数。

**router**

`router.METHOD(path, [callback, ...] callback)`   
METHOD可以为get,post,put等，与app方法同理

**使用了node 的fs文件系统**

fs.readFile(path[,options],callback)     读取文件，回调中有err和data的参数
fs.writeFile(path[,options],callback)     写文件，回调中有err的参数

 **实例**

 下面是专门写接口的js文件，因为太长，所以只列出了获取和添加
 
```javascript
const express = require('express');
const router = express.Router();
const fs = require('fs');
const path = require('path');
//json文件所在路径
const stuAdd = './../data/students.json';

// 获取数据
router.get('/', function (req, res) {
    //读取文件，并返回给浏览器
    fs.readFile(path.join(__dirname + stuAdd), 'utf8', function (err, data) {
        if (err) {
            return res.send(err)
        }
        res.status(200).send(data)
    })
})
// 创建
router.post('/create', function (req, res) {
    //设置id等数据
    let newS = {
        id: Date.now(),
        name: req.body.name,
        age: req.body.age,
        gender: req.body.gender,
        clazz: req.body.clazz,
        address: req.body.address,
        creat: new Date().toLocaleString(),
        update: new Date().toLocaleString()
    };
    //读取文件并把新的内容传入json文件
    fs.readFile((__dirname + stuAdd), 'utf8', function (err, data) {
        //如果有错，抛出错误，防止格式错误
        if (err) throw err

        try {
            data = JSON.parse(data)
        } catch (error) {
            return res.status(200).send(error)
        }

        data.push(newS)
        //把新数据传入json文件
        fs.writeFile(path.join(__dirname + stuAdd), JSON.stringify(data, null, 4), function (err) {
            res.status(200).send(data)
        })
    })
})
//导出整个页面
exports = module.exports = router;
```
在总的js文件中引入该文件，并配置，下面是总的文件

```javascript
const express = require('express');
const app = express();
const studentRo = require('./router/student.js')
//请求体默认为undefined，当解析时才会填充，所以要使用body-parser
app.use(express.urlencoded());
app.use(express.json())
// 静态文件
app.use(express.static('public'));
//跨域
app.use(function (req, res, next) {
    res.header('Access-Control-Allow-Origin', '*')
    res.header('Content-Type', 'application/json')
    next();
});
app.listen(5000, console.log('Server is running on port 5000'))
app.use('/student',studentRo)
```
