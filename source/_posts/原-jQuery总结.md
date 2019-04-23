---
title: '[原]jQuery总结'
categories:
- 前端
tags:
- jquery
date: 2018-12-21 20:54:42
---

## jQuery入口函数
`$(document).ready(function(){})`

`$(function(){})`

## js入口函数与jQ入口函数的区别
js函数的入口函数比jq慢，因为js的入口函数会等待页面加载完成，并且外部资源也加载完成后才会执行，但jq
的入口函数等待页面加载完成后就会执行。
js入口函数只能执行一次，会产生覆盖，但jq的可以多次使用

## 隐式迭代
jQuery有隐式迭代的特性，会自动的遍历，设置操作时会给选择到所有的对象都设置上相同的值。在获取值时，如果选择的对象中有多个元素，就只会获取到第一个的值，因为隐式迭代会让jq认为所有的值都为相同的。

## 链式编程
jq的方法都会返回一个对象，所以我们可以一直调用下去。但返回的如果不是当前对象，就不能链式下去。

## 选择器

**基本选择器**

 1. ID选择器

 2. 类选择器

 3. 标签选择器

 4. 并列选择器    `$("p,span")`

 5. 交集选择器    `$("p.box")`

 
**层次选择器**

 1. 直接子级选择器

 2. 后代选择器

 3. 后面所有同级  `$("p~span")`

 4. 后面紧挨着的同级  `$("p+span")`
 
 **筛选选择器（方法）**   ()里面也可以加选择器

  `.first()       `            第一个元素

  `.last()        `           最后一个

  `.children()    `        相当于子级选择器

  `.find()        `           相当于后代选择器

  `.parent()      `         父元素

  `.parents()     `        祖先元素

  `.next()        `          下一个同级

  `.nextAll()     `         下面所有的同级

  `.nextUntil()   `         往下找，直到选择器为止不包括`选择器（相当与一个区间）

  `.prev()        `          上一个 

  `.prevAll()     `         上面所有的同级

  `.prevUntil()   `        往上找，直到选择器，不包含选择器

  `.siblings()    `         除自身外所有的同级

  `.slice()       `           同数组方法

  `.filter()      `            选取满足选择器的元素

  `.not()         `           选取不满足选择器的元素

  `.has()         `          选取一个元素中有子元素满足此`选择器

  `.is()   `            判断是否有此元素，如果一个`有返回true，如果有多个，返回集合

  `.closest()     `        从自身找距离自己最近的元素

  `.offsetParent()`     距离自己最近的定位祖先元素

  `.eq(index)     `       选取指定下标的元素

  `.get(index)`方法与此相同，但get()返回的是DOM对象，所以也可以用来将jq对  象转为DOM对象  ,$(dom对象)这种方法可以转为jq对象
  
  **过滤选择器**

  `：first`                第一个

  `：last`                最后一个

  `：even`              偶数个
  
  `：odd`                奇数个

  `：gt(index)`    大于索引的

  `：lt(index) `        小于索引的

  `：contains()`      选取内容满足选择器的

  `：not`                不满足选择器的

  `：visible `          可见的元素

  `：hide`               隐藏的元素

  `：parent `          非空元素

  `：empty `           空元素

  `：has()`              选择包含有选择器的元素

  `：animated`        选择正在应用动画状态的元素

  `：target `            选择锚链接的目标位置

  `：button`

  `：checked`

  `：selected`

  `：disabled`

  `：enabled`

  所有css3的子元素选择器，属性选择器，这里就不过多写了（nth-child等）
  
 **使用上下文获取元素**

`$("p","#box")` 从#box下获取p元素
 
## 操作样式

**css()**

传一个参是获取样式值

传一个属性一个值是修改或添加

操作多个可以传对象

**操作class**

`.addClass()`   添加类

`.removeClass()` 移除类

`.hasClass()`   判断是否有这个类

`.toggleClass()` 切换类，如果有则删除，如果没有则加上

## 操作属性

**attr()**

与css的传参方式相同

`removeAttr()`  删除属性

**prop()**

操作一些属性值为布尔值的属性，例如"checked,disabled等

`removeProp()`  删除属性

**data()**

操作自定义属性值

`removeData()`   删除属性

## 动画

`show()和hide()`   显示和隐藏    第一个参数是动画执行时间，第二个回调函数

`slideDown(),slideUp(),slideToggle()`    划入滑出和切换   

`fadeIn,fadeOut,fadeToggle`       渐入，渐出和切换（改变透明度）

`animated(property,执行时间，执行效果（swing/linear），回调函数)`

**动画队列**

jq会把动画储存到动画队列中，动画会按照顺序执行，不会丢失。但如果多次快速触发动画，动画执行需要一定时间，当不触发动画时，动画依然会执行，一直等到动画队列结束，为了解决这个问题我们可以使用 stop()方法

`stop()`方法要在动画前调用，当触发动画时，先停止之前动画，再执行动画效果。

`stop()`的方法有两个参数，第一个为是否清除动画队列，第二个为是否跳到动画的最终效果，一般情况下我们不会使用这两个参数。

`delay`(延迟时间，[队列名称])`   推迟动画队列之后的项目

## 节点操作（增删改复制）

创建：`$()`

添加：`append()` 添加到目标元素的最后面（作为子元素）    也可以把想要创建的元素直接写在括号中，会先创建后添加

`appendTo()`   与上的不同在于：目标元素写在括号中

`prepend ()` 添加到目标元素的子元素的最前面

`prependTo()`   与 appendTo同理

`after()`              添加到目标元素的下面（作为兄弟元素）

`innerAfter() `   

`before()`               添加到目标元素的上面（作为兄弟元素）

`innerbefore()`

替换：`replaceWith() `    把目标元素替换为括号中的元素

`replaceAll()`         用括号中的元素替换目标元素

克隆：`clone(true)`    深复制，不仅复制元素本身及其子元素，还会复制其绑定的事件和数据

`clone(true)`    浅复制，只复制元素本身及其子元素，不会复制事件及数据

`clone(true，false)`   复制元素本身及其子元素，但只复制父元素的事件及数据，不复制子元素的事件及数据
只要第一个参数为false，则不管第二个参数是什么，都为false

删除：`empty()`     清空元素，但元素自身依旧存在。会解除绑定事件

`html('')`       清空元素，但元素自身依旧存在。但不会清空被清空的元素身上绑定的事件，会造成内存泄露。

`detach()`删除元素，自身也会被删除。但此方法不会删除元素身上绑定的事件数据

`remove()`    删除元素，自身也会被删除。这个方法会删除绑定的事件数据。
删除的这两个方法，都不会删除jq对象，都会返回被删除的元素。我们可以在需要的时候重新添加到页面中。
           
## 事件的注册触发解绑

注册：`on(事件类型，[事件委托的子元素]，[传递给事件处理函数的数据，一般使用event,data来回去]，事件处理函数)`

触发：`trigger(事件类型)`

解绑：`off(事件类型)`

##  特殊属性操作（属性值，高宽）

获取或修改表单中的值：`val()`   传值为修改

获取或修改标签中的值：`text()`   `html()`  与`innerText`和`innerHTML`相同

`width()`   实际宽度width,返回一个数字，传值可修改

`height()`   实际高度

`innerWidth()`  width+padding

`innerHeight()`   height+padding

`outerwidth()`    width+padding+border

`outerHeight()`   height+padding+border

`outerwidth(true)`    width+padding+border+margin

`outerHeight(true)`   height+padding+border+margin

`scrollTop()`     被卷曲出去的高度

`scrollLeft()`     被卷曲出去的宽度

`offset()`     获取元素距离document的距离，返回一个对象，里面有top和left的值。如果设置传一个对象，设置之后会给元素加相对定位，改变位置。

`position()`    获取元素距离有定位父元素的距离，只可获取不能修改

## 操作集合

`add()`   合并集合，括号内可以是选择器，标签，DOM对象，jq对象

`addBack()`   将过滤后的对象与上一个合并

`contents()`   获取对象中每一个元素的内容（文本，注释和标签）

`index()`     返回当前元素在所有兄弟元素里面的索引

`end()`   返回到上一次的操作


##  $冲突

当jq拿到控制权时，可以使用onConflict()来释放控制权，如果没有控制权可以使用jQuery来代替$

## 包裹元素
`wrap()`   使用一个元素包裹每一个被选择到的对象

`wrapAll()`   使用一个元素包裹所有被选择到的元素

`wrapInner()`   使用指定标签包裹元素中的内容

`unwrap()`    删除包裹该元素的元素，也就是父元素，如果括号内传了选择器，如果不满足此选择器，则不会被删除

## 工具方法

`$.each(function(索引,属性){})`   

`$.each(function(属性名，属性值){})`   遍历

遍历对象时形参为属性名和属性值，数组或jq对象时，形参为索引和属性

`$.extend(true/false,obj1,obj2)`  将第二个及其以后的对象合并到第一个  第一个值为true是深合并，会合并对象中的对象，false则不会。如果对象中的属性名有相同的，后面的会覆盖前面的。如果属性值出现undefined，则会被忽略。
可以用来复制对象   `$.extent({},obj1)`

`$.greg(数组，function(属性，索引){})`           从数组中过滤出满足条件的元素，返回值为布尔值

`$.merge(arr1,arr2)`把第二个数组合并到第一个，这个方法只能有两个参数

`$.inArray(要找的元素，去哪个数组找，从索引几开始找（默认0）)`     属性是否在数组中，是则返回索引，不是返回-1

`$.map(数组/对象，function(元素/属性值，索引/属性名){})`    用指定函数处理对象或数组中的每一个元素，返回值作为数组或对象中的元素，并把结果封装为新的数组或对象，如果返回null/undefined，则不会被添加到数组或对象中

`$.makeArray()`     把一个类似数组（有集合，下标）的对象转为数组

`$.isArray()`     判断是否为数组

`$.isPlainObject()`  判断是否为一个对象

`$.isEmptyObject()`   判断是否为空对象

`$.type()`检测数据类型   与typeof()类似

`toArray()`   将JQ对象转为数组

## AJAX
`$().load(url，与请求一同发送的查询字符串键/值对集合，回调函数)`   从服务器加载数据，并把返回的数据放置到指定元素中

`$().getScript(url,回调函数)`   使用get方式获取js类型的数据

`$().getJSON(url,回调函数)`    使用get方式获取quJSON类型的数据

`$().get(url,回调函数,期望返回数据的类型)`  使用get方式获取指定类型的数据   能接受数据的类型有5种：html,text,JSON,js,xml

`$().post(url,提交的数据，回调函数,期望返回数据的类型)`  使用post方式提交数据 

`$().ajax(url,各种设置)/ $().ajax(各种设置)`    设置：url，data数据，dataType数据类型，timeout超时时间，method提交方式，global ：true/false是否触发全局ajax事件处理程序，success回调函数，error回调，complete回调，beforeSend回调

`$().ajaxsetup()`  设置下面的ajax请求的默认值


