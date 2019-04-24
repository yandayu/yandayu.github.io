---
title: '[原]flex box布局'
categories:
- 前端
tags:
- css
date: 2018-11-26 20:31:14
---

给父级元素设置display：flex会使之转成弹性盒模型，为我们提供了简单方便的设置盒子水平垂直居中的方法。

 1. 设置主轴（水平）方向居中

    justify-content :   
    flex-start  （默认值主轴起始点位置对齐（水平方向左对齐）

    flex-end      主轴的结束位置（水平方向右对齐）

    center          居中对齐

    space-between    两端对齐，项目之间的间隙相等

    space-around    项目之间的间隙比项目到边框的间隙大一倍，与 space-between   的区别在于此方式两端会有一定的空隙，而 space-between 项目是紧挨着边框的

    ![justify-content 对齐图解](https://raw.githubusercontent.com/yandayu/yandayu.github.io/img/images/justify-content.png)


 2. 设置交叉轴（垂直）上的对齐方式

    align-items：    stretch （默认值）  当子元素盒子没有设置高度或设置为auto时，垂直方向自适应，也就是高度会填充满父元素

    flex-start      交叉轴起始点位置对齐

    flex-end       交叉轴的终点对齐

    center          交叉轴的中点对齐

    baseline       基于第一行文字的基线对齐

    ![align-items对齐图解](https://raw.githubusercontent.com/yandayu/yandayu.github.io/img/images/align-items.png)

 3. 多行沿交叉轴对齐（多行轴线的对齐方式）**如果只有一根轴线，不起作用**
        align-content :        flex-start      与交叉轴的起点对齐

        flex-end       交叉轴的终点对齐

        center          交叉轴的中间对齐

        space-between    交叉轴两端对齐，均匀分布

        space-around        多行中行与行之间的上下距离是最上或最下一行距离上下边框距离的一倍，也就是相当于上图倒过来的效果

 
那么如果想设置一个盒子垂直水平居中的话就可以给他的父级元素设置： `display：flex                                       justify-context : center                             align-item : center`
