---
title: 'jwt'
categories:
- 前端
tags:
- jwt
date: 2019-01-29 13:08:27
---

`jwt`是jsonwebtoken的简称，用来生成token。在登陆状态验证完成时发送给前台一个token，之后前台的请求都要带着token，后台验证其token的正确性及有效性之后才会回应其数据，在用户和服务器端之间安全的传递消息。

**jwt实际是一个字符串，由三部分组成，头部，数据和签名。**

生成token:`const jwt = require('jsonwebtoken')`

我们在页面引入jsonwebtoken后，可以使用其方法sign来生成token:`jwt.sign(payload,secret,[options,callback])`

sign方法有四个参数:
1. 生成的token中包含的一些数据，此参数可以是一个对象，字符串或buffer。如果是对象，会使用JSON.stringify()转为json格式。我在对象中写入了一个GUID(时间戳)和user-agent信息;
2. 一个密钥，记录在服务器中，在验证时需要用到此参数;
3. 一些选项，经常用的大概就是有效期(expiresIn)，有效期的值以秒计数，也可以使用一些带有时间跨度的字符串，eg：’7d’,’5h’等;
4. 一个回调，回调的第一个参数是err,第二个是token。但此方法会默认返回token;

**下面是贴了实例的代码，在其中还使用了level存储了一个状态和时间**
```javascript
const jwt = require('jsonwebtoken');
const level = require('level');
// 创建实例
var db = level('./mydb')

// 密钥
var serect = 'yandayu'


// 获取GUID
function generateGULD() {
    return new Date().getTime()
}

// 生成token
function generateToken(req, GUID, opts) {
    opts = opts || {}
    // 默认有效期为7天
    var expiresDefault = '7d';
    
    var token = jwt.sign({
        auth: GUID,
        agent: req.headers['user-agent']
    }, serect, { expiresIn: opts.expires || expiresDefault })

    return token;
}

function generateAndStoreToken(req, opts) {
    var GUID = generateGULD();
    var token = generateToken(req, GUID, opts)

    var record = {
        valid: true,
        created: new Date().getTime()
    }
    // 使用level把数据插入存储区
    db.put(GUID, JSON.stringify(record), function (err) {
        console.log(err)
    })

    return token;
}
exports = module.exports = {
    generateAndStoreToken
};
```
**验证token**
在之后前台的操作中需要传回token，后台进行验证。
```javascript
// 使用`jwt.verify` 方法传入token和密钥，如果有效会返回当时使用sign方法传入的第一个参数,通过验证其是否返回的是有效参数来确认其token是否有效。
function verify(token){
    var decoded = false;
    try {
        decoded = jwt.verify(token, secret);
       
    } catch (error) {
        decoded = false;
    }
    return decoded;
}
// 校验token的中间件方法
function checkToken(req,res,next){
    var token = req.headers.authorization;
    var decoded = verify(token);

    if(!decoded || !decoded.auth){
        res.checkToken = false;
        return next()
    }else {
        // 并且在当时使用了level把GUID作为键值对的键，所以可以通过verify方法返回数据中的GUID来获取值中存储的有效与否来再次检验是否有效。
        db.get(decoded.auth,function(err,record){
            try {
                record = JSON.parse(record)
            } catch (error) {
                record = { valid: false };
            }
            if(err || !record.valid){
                res.checkToken = false;
                return next()
            }else{
                res.checkToken = true;
                return next()
            }
        })
    }
        // 在登出操作时把record.valid改为false，再重新写入即可。
}
```
