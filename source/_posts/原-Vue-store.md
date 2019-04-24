---
title: '[原]Vue-store'
categories:
- 前端
tags:
- vue
date: 2019-01-18 12:57:54
---

## 使用store模式

用store模式写一个周记，图示如下：

![周记](https://img-blog.csdnimg.cn/20190423125621550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjk3MDcy,size_16,color_FFFFFF,t_70)

实现编辑删除，添加等功能，点击某天，下面添加处Day of event:显示的为对应星期。

使用store就是把数据(这里是假数据)托管到store的js文件上，各个组件中修改，删除等操作，不直接对数据进行操作。而是通知store去操作，所有方法写在store 文件中，在操作时调用即可。在项目简单时使用store模式即可，在项目较为复杂时可以使用vuex。

下面为store文件的代码：

```javascript
import {seedData} from './seed.js'

//状态管理，页面中有任何改变或查询时，都通知store进行改变，而不在页面进行改变
export const store = {
// 托管数据
state:{
    seedData
},   
// 获取激活状态的天数
getActiveDay(){
    return this.state.seedData.find(day => day.active)
},
// 点击哪天，把哪天设置为激活状态
setActiveDay(dayID){
    this.state.seedData.map(function(day){
        day.id == dayID ? day.active = true :day.active = false
    });
},
// 为激活状态天数添加事件
addEvent(message){
    if(message) {
        this.getActiveDay().events.push({ details: message, edit: false })
    }  
},
// 封装获取指定天数，指定事件，及事件索引
getEventDetails(dayId, details){
    const dayObj = this.state.seedData.find(day => day.id == dayId );
    var resObj = {
        dayObj
    }
    dayObj.events.forEach(function(event,index){
        if(event.details == details) {
            resObj.event = event
            resObj.index = index
        }    
    })
    return resObj;
},
// 点击编辑，将事件处于编辑状态
editEvent(dayId, eventDetails){
    // 将其他编辑状态都变为false
    this.state.seedData.map(function (obj) {
        obj.events.map(event => event.edit = false)
    })
    this.getEventDetails(dayId, eventDetails).event.edit = true;
},
// 删除指定事件
deleteEvent(dayID, eventDetails){
    let delObj = this.getEventDetails(dayID, eventDetails)
    if (delObj.index >= 0){
        delObj.dayObj.events.splice(delObj.index,1)
    }
},
// 修改指定事件
changeEvent(dayId, oldEventDetails, newDetails){
    // 如果新数据为空，则不修改，把旧数据赋值给新数据
    if (newDetails == '') newDetails = oldEventDetails
    const changeEvents = this.getEventDetails(dayId, oldEventDetails).event
    changeEvents.details = newDetails
    changeEvents.edit = false
}
}
```