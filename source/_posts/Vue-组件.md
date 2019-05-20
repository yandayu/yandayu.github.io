---
title: 'Vue-组件'
categories:
- 前端
tags:
- vue
date: 2019-01-16 21:29:37
---

**注册组件**

```javascript
        //要先注册再实例，此为全局组件
		Vue.component('button-counter',{
		 	//data必须是一个函数
		 	data:function(){
                return {
                 	count:0
                 }
	 	    },
		 	template:'<button type="button" @click="count++">点击了{{count}}次</button>'
		});
       //局部组件
		new Vue({
			el:'#app',
			components:{
				//在根组件下注册一个父组件，只能在#app下使用
				'parent-button':{
					//在父组件下定义一个子组件，只能在父组件下使用
					components:{
						'child-button':{
							data:function(){
								return {count:0}
							},
							template:'<button type="button" @click="count++">点击了{{count}}次</button>'
						}
					},
					//注册子组件
					template:'<child-button></child-button>'
				}

			}
		});
```

**组件之间传递数据（父 --> 子）**

```html
	<div id="app">
		<h1>{{info}}</h1>
		<!-- 从根组件往子组件传递动态数据message -->
		<child-component :message="info"></child-component>
	</div>
	<script type="text/javascript" src="vue.js"></script>
	<script type="text/javascript">
		//组件的作用域：每个组件都是独立的，不能相互访问。
		//从父级组件往子级组件中传递数据时，在子组件上面写一个属性，属性值为要传递的数据。如果传递的是动态数据，要绑定此属性，也就是说在属性前加v-bind:简写为：
		new Vue({
			el:'#app',
			data:{
                info:'根组件数据'
			},
			components:{
				'child-component':{
					//用props接数据,还可以进行校验
					props:{
                        'message':{
                            type:String,//(默认为any,也可以自己定义类型，多个类型用数组)
                            required:true,//为true 时必须传值，否则可不传
                            default:''  //默认值
                        }
                    },
					template:'<div><h1>{{message}}</h1></div>'
				}
			}
		});
	</script>
```

**自定义事件(子 --> 父)**

```html
    <div id="app">
           <h1>{{info}}</h1>
           <!-- 自定义事件 -->
        <child-component @pass-mess="changeMessage"></child-component>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">
       
        new Vue({
            el: '#app',
            data: {
                info: '根组件数据'
            },
            components: {
                'child-component': {
                    data:function(){
                        return {childMessage:'子组件的数据'}
                    },
                    template: '<div><h1>{{childMessage}}</h1></div>',
                    //初始化后获取子组件的数据
                    created: function () {
                        //触发自定义事件，并传入子组件的数据
                        this.$emit('pass-mess', this.childMessage)
                    }
                }
            },
            methods: {
                // 自定义事件触发的函数
                changeMessage:function(message){
                    this.info = message
                }
            }
        });
    </script>
```
我们也可以直接通过实例属性来访问

`.$children`  子组件，是一个数组，可以通过索引拿

`.$parent`    父组件，只有一个

`.$root`      根组件

**子组件的索引**

我们如果复用了多次子组件，通过实例属性索引来拿不确保能拿到相应顺序的子组件。所以我们可以为每个组件添加一个特有的索引，使用 `ref="索引名"`。通过`.$refs.索引名`来准确拿到我们想拿的组件。

**使用插槽分发内容**

```html
    <div id="app">
        <child>
            <!-- 标签会插到指定名字的插槽中 -->
            <span slot="slot1">{{rootMessage}}</span>
            <p>{{count}}</p>
            <!-- 使用对象解构的形式接收一个对象 ,也相当于从子组件往父组件传递了数据-->
            <template slot-scope="{stu}" slot="slot2">
                <h1>{{stu.name}}</h1>
                <h1>{{stu.age}}</h1>
            </template>
        </child>    
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="x-template" id="childTemplate">
        <!-- 模板中的内容会覆盖child标签及内容,如果不想覆盖就要使用插槽solt -->
        <div>
            <slot name="slot1"></slot>
            <!-- 没有name的为默认插槽，所有没有slot属性的标签都会被加到这个插槽中 -->
            <slot></slot>
            <!-- 从子组件传递一个对象 -->
            <slot :stu="student" name="slot2"> </slot>
        </div>
    </script>
    <script type="text/javascript">

        new Vue({
            el: '#app',
            data:{
               rootMessage:'根组件',
               count:1
            },
            components: {
                'child': {
                    data:function(){
                        return {
                            student: {
                                name: 'lisa',
                                age: 20
                            }}
                    },
                    template: '#childTemplate'   
                }
            }
        });
```
**动态组件**

```html
    <div id="app">
         <a href="#" @click.prevent="page='index'">首页</a>
         <a href="#" @click.prevent="page='about'">关于我们</a>
         <a href="#" @click.prevent="page='contract'">联系我们</a>
         <!-- 动态组件 is的值绑定为哪个组件就显示指定组件的模板，component相当于一个占位符，不会输出
              但切换时，动态组件都会重新渲染，所以组件的状态不会被保存，我们可以使用keep-alive使其保持状态 
              include里面的组件会保持状态，exclude里面的不会-->
              <keep-alive include="index,about" exclude="contract">  
                    <component :is="page"></component>
              </keep-alive>
    </div>
    <script type="text/javascript" src="vue.js"></script>
    <script type="text/javascript">
        new Vue({
            el: '#app',
            data:{
               page:'index'
            },
            components: {
                'index':{
                   template:'<div><h1>首页</h1><input type="text"></div>'
                },
                'about': {
                    template: '<div><h1>关于我们</h1><input type="text"></div>'
                },
                'contract': {
                    template: '<div><h1>联系我们</h1><input type="text"></div>'
                }
            },
        });
    </script>
```