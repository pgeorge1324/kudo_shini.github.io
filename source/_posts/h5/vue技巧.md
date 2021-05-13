---
date: 2021-05-13 14:26:52
title: Vue技巧
tags:
- h5
- vue
categories:
- [h5, vue]
---

# Vue技巧

- ### 添加滚动事件

  ```vue
  mounted{
  	this.$refs.boxScroll.addEventListener('scroll',this.handleScroll) 
  	// 监听滚动事件，然后用handleScroll这个方法进行相应的处理
  },
  beforeDestroy(){
  	this.$refs.boxScroll.removeEventListener('scroll',this.handleScroll) 			// 取消监听滚动事件，需要在beforedestroy
  },
  
  ```


- ### 对象的监听与修改

  ```vue
  this.$set(data,key,newValue) //只能用于给复杂数据添加属性触发监测
  this.$forceUpdate(); //用于强制刷新
  ```

- ### 面向vue组件的节流函数

  ```vue
  /**
       * [throttle js节流公用方法]
       * @param  {[function]} method  [你要执行的方法]
       * @param  {[]} context [上下文语境如window，可忽略]
       */
      throttle(method, context, t,...args) {
          clearTimeout(context.timerId);
          t = t || 200;
          context.timerId = setTimeout(() => {
              method.apply(context,args);
          }, t);
      },
  ```

  
  