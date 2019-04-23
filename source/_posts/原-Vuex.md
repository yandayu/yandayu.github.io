---
title: '[原]Vuex'
categories:
- 前端
tags:
- vue
date: 2019-01-18 20:59:54
---


## Vuex

**在使用@vue/cli脚手架搭建时，选择vuex后会生成store.js文件，并且已经做了引入Vue，Vuex,实例Vuex等基本的操作。**

   ```javascript
   //  实例有五个选项
export default new Vuex.Store({
  state: {
    //  用来托管数据
    notes: ['one', 'two']
  },
  mutations: {
    //  用来直接更改state中数据的状态，同步
    //  可以定义多种方法，每个方法中会传入两个值，第一个为state，第二个为传入的数据
   ADD_NOTE (state, payload) {
      state.notes.push(payload)
    }
  },
  actions: {
    //  用来像mutations提交，但不直接更改数据，可以保护异步操作
    //  context是一个与store实例具有相同方法和属性的对象,payload为传入的数据
    addNote (context, payload) {
       //  向mutations中ADD_NOTE提交数据
      context.commit('ADD_NOTE', payload)
    }
  },
  getters: {
    //  与访问器属性的get方法类似，用来提供数据
    notes: state => state.notes
  },
  module: {
    // 可以把store分割多个模块，每个模块中都有自己的state，mutations，actions，getters，module
})
##通过dispatch触发action中的方法##

// 当实例后根组件及其子组件中都会被注入$store属性，通过此属性来访问实例中的内容
this.$store.dispatch('addNote', this.message)
##辅助函数##

// 在分模块时store的方法较多，可以使用此辅助函数把各个模块的getters和actions的方法展开在组件中，不用再通过this.$store.getters/dispatch()的方法来调用
   import {mapGetters, mapActions} from 'vuex'   // 引入方法
    computed:{
       ...mapGetters(['cartItems'])
   },
   created(){
       // 直接调用
     this.getCartItems()
   },
   methods:{
       // 把vuex的store中获取商品和清空的方法，展开在此组件自有方法中，可以直接调用
     ...mapActions(['getCartItems','deleteAll'])
   }
   ```