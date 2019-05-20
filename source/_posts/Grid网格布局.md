---
title: 'Grid网格布局'
categories:
- 前端
tags:
- grid,css布局
date: 2019-05-12 21:23:51
---

网格线：网格容器的任意一条线，分为列网格线与行网格线

网格轨道：两条相邻的网格线之间的空间为网格轨道

网格单元格：两条相邻列网格线和两条相邻行网格线包围的空间为单元格，也就是表格的一个格

网格区域：四条网格线包围的总空间为网格区域，一个区域中会包含多个单元格

隐式轨道：当网格中的网格项对于单元格时，或位于显式网格之外时，就会创建隐式轨道

## 父元素/网格容器

 `display:grid/inline-grid(内联网格)` 将元素设置成网格容器

`grid-template-columns/gris-template-rows` 列/行的宽/高，也就是网格轨道的大小

`repeat()` 定义多个重复值，第一个参数为重复几次，第二个是重复项]
`minmax()` 第一个参数表示最小轨道尺寸，第二个为最大尺寸。

`fr` 单位允许使用等分容器的剩余可用空间来设置网格轨道的大小


```cssgrid-template-columns:repeat(3,1fr)```
等价于
```css grid-template-columns:1fr 1fr 1fr ```
也就是每列占容器可用空间的1/3

`grid-template-areas` 通过引用grid-area属性来指定的网格区域来定义网格的可视化结构
```css
.main{
   display:grid;
   grid-template-columns:30px 30px 30px 30px;
   grid-template-areas:"header header header header"
                       ".      body   body   body"
                       "none footer  foooter none"
}
.head{
   grid-area:header;
}
.body{
   grid-area:body;
}
.foot{
   grid-area:footer;
}
```
以上会创建一个4列3行的网格容器，总宽度为120px,每列30px,header独占一行，.代表空单元格，none为无

`grid-template` 是`grid-template-columns/rows/areas`的简写，但此属性不会覆盖三个属性

`grid-column-gap/gris-row-gap` 指定列/行之间的间隙与margin类似，但边缘不会有间隙，grid-可省略

`grid-gap`为上一条的缩写，`<row><column>`,只写第一个时，第二个同第一,grid-亦可省略

`justify-items` 沿着行轴线对齐网格项

可选值： 
1. start 将网格项对齐到其单元格的左侧起始边缘
2. end  对齐到其单元格的右侧
3. center 水平居中
4. stretch 填满单元格的宽度（默认）

 `align-items` 沿着列轴线对齐网格项

 可选值：
1. start 将网格项对齐到其单元格的顶部起始边缘
2. end  对齐到其单元格的底部
3. center 垂直居中
4. stretch 填满单元格的高度（默认）

`place-items` 为以上两个的简写，第一个为`align-items`

`justify-content` 在网格大小小于网格容器大小时，设置网格容器内的网格沿着行轴线对齐方式

可选值： 
1. start 将网格项对齐到其单元格的左侧起始边缘
2. end  对齐到其单元格的右侧
3. center 水平居中
4. stretch 填满网格容器的宽度
5. space-around 每个网格项之间有均匀的间隙，左右两端一半的间隙
6. space-between 每个网格项之间有均匀的间隙，左右两端无间隙
7. space-evenly 每个网格项之间有均匀的间隙，左右两端也是均匀的间隙

`align-content` 沿着列对齐网格，参数同上

`place-content` 以上两个的简写，先列后行

`grid-auto-columns/rows` 任何自动生成的网格轨道大小（隐式轨道大小） 

`grid-column/row` 用来定位网格项

当有一个两行两列的网格时
```css
.a{
   grid-column:5/6;(第五条列网格线开始，第六条结束)
   grid-row:2/3;
}
```
.a引用的网格线不存在，会自动生成隐式网格轨道来填补空缺，可以使用`grid-auto-columns/rows`来指定他们的大小

`grid-auto-flow` 如果不指定网格项的位置，自动算法会自动放置这些网格项，此属性会控制自动布局算法

可选值：
1. row 依次填入每行，根据需要添加新行（默认）
2. column 依次填入每列
3. dense 在稍后出现较小的网格项时，尝试填入网格中较早的空隙中

`grid` 简写,左行右列

1. `grid-template`
2. `[auto-flow&&dense?]<grid-auto-rows>?/grid-template-columns`

eg:`grid:auto-flow 100px/1fr 1fr`
等价于：
```css
grid-auto-flow:row;
grid-auto-rows:100px;
grid-template-columns:1fr 1fr;
```

3. `grid-template-rows / [auto-flow&&dense?]<grid-auto-columns>?`

> 一次设置所有：
```css
.main {
  grid: [row1-start] "header header header" 1fr [row1-end]
        [row2-start] "footer footer footer" 25px [row2-end]
        / auto 50px auto;
}
```
等价于
```css
.main {
  grid-template-areas: 
    "header header header"
    "footer footer footer";
  grid-template-rows: [row1-start] 1fr [row1-end row2-start] 25px [row2-end];
  grid-template-columns: auto 50px auto;    
}
```

## 子元素/网格项属性

`grid-column-start/grid-column-end/grid-row-start/grid-row-end` 通过网格线来确定网格项的位置

可选值：
1. 网格线的编号或名字
2. `span 数字/名称` ：网格项将跨越几个网格轨道/跨越到网格线名称位置
3.  auto：自动放置，自动跨度，默认1个网格轨道

`grid-column/grid-row` 上面的简写

值为：开始的线 结束的线 / `span <value>`

`grid-area` 为网格项提供一个名字，也可当作以上的简写

值：name| row-start / column-start / row-end / column-end

`justify-self` 沿着行对齐网格项，适用于单个网格项中的内容

可选值： 
1. start 将网格项对齐到其单元格的左侧起始边缘
2. end  对齐到其单元格的右侧
3. center 水平居中
4. stretch 填满单元格的宽度（默认）

 `align-self` 沿着列轴线对齐网格项

 可选值：
1. start 将网格项对齐到其单元格的顶部起始边缘
2. end  对齐到其单元格的底部
3. center 垂直居中
4. stretch 填满单元格的高度（默认）

`place-self` 以上两个的简写，第一个为`align-self`


