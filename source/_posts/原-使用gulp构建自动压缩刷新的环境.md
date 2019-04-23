---
title: '[原]使用gulp构建自动压缩刷新的环境'
categories:
- 前端
tags:
- node
date: 2019-01-09 15:40:12
---

 **基于node的工具，要确保安装了node**

 1. 全局安装gulp 的命令行  npm install gulp-cli -g
 2. 创建一个项目并切到该文件夹中   mkdir   文件名 && cd 文件名
 3. 初始化（会多一个package.json的文件，所有设置及依赖会显示在该文件中），会问你几个关于项目的名称版本等问题，如果使用默     认的，可以再后面加-y，就会直接创建。  npm init  -y
 4. 作为项目依赖安装gulp   npm install gulp -D
 5. 创建一个gulpfile.js的文件，如果文件名不为此名称，需要在package.json中修改main的值为对应的名称。可以在json文件中添加 "private": true ，让项目成为私人的。
 6. 在gulpfile.js文件中引入gulp及所需要的src,dest,pipe,series,watch一系列内置插件
 7. 安装压缩混淆js文件的插件（gulp-uglify），压缩html文件的插件（gulp-htmlmin），压缩css文件的插件（gulp-cssnano），服务器的插件（gulp-connect），都引入到文件中
 8. 创建压缩混淆js文件的任务
 
```javascript
//压缩混淆js   有返回值的不用回调
function script(){
  return src('src/js/*.js')
         .pipe(uglify())
         .pipe(dest('dist/js'))
         .pipe(connect.reload())
}
```

使用src方法找到要压缩混淆的文件，pipe方法为管道传输，传输过程中使用压缩混淆的方法处理后，
（dest方法为输出）处理后输出到dist文件下的js文件下，最后自动刷新
压缩html和css方法除了所用的插件不一样外，都相同。

 9. 任务创建完成后，使用watch方法监听三个任务，任务中如果没有返回值要添加一个回调

```javascript
// 监听文件变更
function watchFile(cb){
    watch('src/**/*.js',script)
    watch('src/**/*.css',css)
    watch('src/*.html',html)
    cb()
}
```

 10. 之后创建服务器的任务，监听dist目录的文件，和8080的端口，保持刷新

```javascript
//实时刷新
function serve(cb){
    connect.server({
        root:'dist',
        port:8080,
        livereload:true
    })
    cb()
}
```

 11. 最后把监听与服务器的任务导出到默认值中，当执行任务时两个任务都会执行
 

```javascript
//导出
exports.default = series(serve,watchFile)
```

 12. 使用node 执行该任务（因为设置了默认值，所以不用写任务名）  gulp ，接下来在浏览器中访问localhost：8080，项目所做的变化，保存页面后就会自动刷新，并且自动压缩。



如果觉得npm下载包太慢，可以使用淘宝镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org
执行此命令后，以后的npm操作就要以cnpm代替npm