---
title: 'Vue-基础语法'
categories:
- 前端
tags:
- vue
date: 2019-01-16 21:28:28
---

**实例与模板语法**

```html
    <div id="app">
         <!--插入文本和标签都会覆盖标签中的内容,{{}}不会覆盖 -->
		<p>{{messages}}</p>    <!--  元素中使用数据   -->
		<input type="text" v-model="message"> 
		<!-- 为input绑定value值，当input改变value值时，message的值也会改变（双向绑定）  -->
        <p v-text="message"></p>    <!-- 插入文本 -->
        <div v-html="tags"></div>   <!-- 插入标签 -->
	</div>
```
```javascript
var app = new Vue({
		el:'#app',    //挂载点，数据只能在挂载点下的元素使用
		data:{
			message:'hello vue!',
			tags:"<span>绑定标签</span>"
		},        //数据
		template:'<h1>{{messages}}</h1>',//模板会与数据结合，之后挂在挂载点上，会替换掉挂载点，所以当有多个元素时，要有一个根元素（父元素）包裹。当模板的代码过多时可以把代码写在<script type="x-template">标签下，给script加个id值，使用：template：'#id值'
		methods:{
			greeting:function(){
				alert(this.message);
			}      //绑定事件：v-on:click="greeting"/@click="message"。不传值时可以不写括号
		},
		computed:{
			messages:function(){
				return this.message + "!"
			}
		}         //计算属性，与js中的访问器属性类似
	})
```
**生命周期方法**

这些方法是在实例各个阶段所触发的

`beforeCreate`  在实例初始化之后，数据观测事件配置之前被调用

`created`       在实例创建完成后被立即调用，未挂载时

`beforeMount`   在挂载开始之前被调用

`mounted `      在挂载完成后立即调用

`beforeUpdate`  数据更新时调用

`updated`       数据更新渲染后调用

`activated`     keep-alive 组件激活时调用

`deactivated`   keep-alive 组件未激活时调用

`beforeDestroy` 实例销毁之前调用

`destroyed`     实例销毁后调用

`errorCaptured` 当捕获一个来自子孙组件的错误时被调用

**操作类名与样式**

```html
<div id="app">
		<!-- v-bind:简写为：      v-on:简写为@ -->
		<!-- 操作类名 表达式为真时加类名-->
		<div :class="{'active':info>=10}"></div>
		<!-- 数组形式 -->
		<div :class="[active?'active':'unactive']"></div>
       <!-- 操作样式 -->
       <div ：style="{witdth:'300px',height:'200px'}"></div>
       <div :style="style"></div>
       <!-- 数组的形式 -->
       <div :style="[style,style2]"></div>
	</div>
	<script type="text/javascript">
		new Vue({
			el:'#app',
			data:{
				info:12,
				active:true,
				style:{
					witdth:'200px',
					height:'200px',
					background:'red'
				},
				style2:{
					witdth:'200px',
					height:'300px',
					background:'green'
				}
			}
		})
    </script>
```
**条件渲染**

```html
<div id="app">
		<!-- 当代码过多时，可以使用template标签，加载时会提取template中的内容到页面 -->
		<div v-if="mess < 10">
			mess<10
		</div>
		<div v-else-if="mess == 10">
			mess == 10执行
		</div>
		<div v-else>
			mess>10
		</div>
		<!-- 两者区别：v-if只会在满足条件时开始渲染，并且在切换时会销毁绑定数据等，条件满足时再重建
		              而v-show在页面加载就会渲染，只是单纯的改变元素的css属性display的值。
		              在频繁切换时使用v-show比较好，条件很少改变时使用v-if较好-->
		<div v-show="20<10">
			v-show
		</div
```

**列表渲染**

```html
    <div id="app">
		<!-- v-for可以渲染的有数组，对象，数字。字符串 
		     语法为item in items
		     为了给Vue一个提示，给每个元素加上一个唯一的key值，防止复用。
		     不要与v-if一同使用,当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。如果要使用，可以把要遍历的数组/对象等替换为一个计算属性，过滤后在遍历添加元素-->
			<ul >
				<li v-for="(student,index) in students">{{index}}-{{student}}</li>
			</ul>
            <!-- 当遍历对象时，形参为属性值，属性，索引 -->
            <div >
				<h2  v-for="(value,key,index) in personInfo" :key="index">{{key}}-{{value}}</h2>
			</div>
    </div>
    <script type="text/javascript">
		var app = new Vue({
			data:{
				students:['Lisa','pola','rose'],
				personInfo:{
					name:'一班',
					age:20,
					gender:'女',
					address:'郑州'
				}
			}
		})
    </script>
```

**事件处理**
绑定事件使用`v-on:事件类型 = ""`的形式,括号里可以写简单的代码或者函数名，不需要传参时可以不加括号。如果传参的形式，函数内需要用事件对象，要把$event以参数的形式传入。v-on:可以简写为@

```html
<a  @click.stop="alert('hey')"></a>
<!-- .stop 是事件修饰符,用处为停止冒泡。
	  Vue为我们提供了一下几种事件修饰符:
	  .prevent 阻止默认事件
	  .capture 捕获阶段执行
	  .self    事件不是从冒泡或捕获来的，是自身的事件
	  .once    只触发一次 
	  按键修饰符：
	  .exact   当只有绑定的按键独自按下时触发，组合按下不会触发
	  .left  .right  .middle
	  表单修饰符:
	  .trim    前后去空格
	  .number  转为数字
	  .lazy    当内容改变触发焦点触发(change)  -->
```

**表单值绑定**

```html
	<label>婚否:<input type="checkbox" true-value="已婚" false-value="未婚" v-modal="marry"></label>
	<label>男<input type="radio" :value="male" v-modal="gender"></label>
	<script type="text/javascript">
		var app = new Vue({
			data:{
				marry:false,
				male:'男'
			}
		})
    </script>
```

**监视数据的变化**

```javascript
    new Vue({
            el: '#app',
            data: {
                name:'张三',
                age:20,
                phone:{
                    brand:'OPPO',
                    number:12354
                }
            },
            watch: {
                name:'changeName',//可以传入一个自定义的处理函数
                age:function(newMess,oldMess){
                 //默认的处理函数，有改变前的数据和之后的数据
                },
                phone:{
                    handler:function(newMess, oldMess){},
                    deep:true,   //深度监视，当对象中的属性改变时也会被监视到
                    immediate:true  //打开页面会立即执行一次回调
                },
               'phone.brand': function (newMess, oldMess){}   //监视对象中的某个属性，因为有.所以要加引号

            },
            methods: {
                changeName:function(){
                    //name改变时的处理函数
                }
            }
    });
```