---
title: 初步接触Vue.js
date: 2016-06-13 19:20
categories:
- 阿里web前端框架
tags:
- Vue.js
- Angular.js
toc:
permalink: image-compression
---

  Vue.js是一个数据驱动组件，由于工作需要，现开始学习，并做下学习笔记，以备今后查询解惑

<!-- more -->

## 概述

1. vue.js是一个构建数据驱动的web界面的库，通过简单的api实现 **响应的数据绑定**  和  **组合的视觉组件**                

2. vue.js 的绑定视图DOM的方法与 angualar.js 很相似

3. **v-if**  特性被称作指令。指令带有前缀 **v-** ，代表vue.js提供的特性，它们会给绑定的目标添加响应式的特殊行为。

## 组件的应用模板
         <div id = "app"> 
            <app-nav></app-nav>
            <app-view>
                <app-sidebar></app-sidebar>
                <app-content></app-content>
            </app-view>
         </div>

## 构造器
  ### **MVVM** 模式
         var vm = new Vue({     // MVVM模式中描述的ViewModel
                //选项(数据、模板、挂载元素、方法、生命周期钩子)
         })
  ### 创建可复用的组件构造器
        所有的Vue.js组件其实都是被扩展的vue实例
         var Mycomponent = Vue.extend({
                // 扩展选项
         })
          // 所有的 ‘Mycomponent’ 实例都将以预定义的扩展选项被创建
         var myComponentInstance = new Mycomponent()
## 属性与方法
   ### 代理数据属性（响应）
       每个vue实例都会代理其 **data** 对象里的所有属性
          var data = { a : 1 }
          var vm = new Vue ({
              el: '#example'
              data: data
          })
          vm.a === data.a   // true
   ### 实例属性与方法（带前缀 **$**）
          vm.$data === data  // true
          vm.$el === document.getElementById('example')   // true
          // $watch 是一个方法
          vm.$watch('a', function (newVal, oldVal) {
              //这个回调将会在 'vm.a' 改变后调用
          })
## 实例生命周期
   vue.js没有“控制器”的概念，组件的自定义逻辑都是分割在各种生命周期钩子上，
   包括 **compiled**、**ready**、**destroyed**、**creaded**

![](http://upload-images.jianshu.io/upload_images/2145995-bc1002f101418414.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![生命周期图示](http://upload-images.jianshu.io/upload_images/2145995-5dd367e30395c073.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
