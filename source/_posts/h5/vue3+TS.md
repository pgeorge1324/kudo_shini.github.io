---
date: 2021-05-13 14:26:52
title: vue3+TS 组件导出
tags:
- h5
- vue
categories:
- [h5, vue]
---

# vue3+TS 组件导出

- ## 三种定义方式在Vue3.*+Ts

  - ### **vue2.0中常用方法，不推荐**

    ```javascript
    /*******************************************************************/
    // 【1.直接导出】
    export default{
    	name: 'myComponent'
    }
    /*******************************************************************/
    ```

    

  - ### **配合shims-vue.d.ts进行组件定义和注册**

     **【注意】:导入组件时需加上‘.vue’后缀**

     ```javascript
     import LaMenu from "@/components/Menu.vue";
     
     /*******************************************************************/
     // 【2.使用defineComponent导出】
     import {defineComponent} from 'vue'
     export default defineComponent({
     	name: 'myComponent'
     })
     /*******************************************************************/
     ```

  - ### **目前vue-class-component不支持vue3.0组合API写法，不建议使用**

    ```javascript
    /*******************************************************************/
    // 【3.使用vue-class-component】
    import Vue from 'vue'
    import Options from 'vue-class-component'
    
    // Define the component in class-style
    // 最新版Component注解已经改为Options
    @Options({
      computed: mapGetters([
        'posts'
      ]),
    
      methods: mapActions([
        'fetchPosts'
      ])
    })
    export default class Counter extends Vue {
    	property: number;
    	method(){}
    }
    /*******************************************************************/
    
    ```

