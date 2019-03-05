## vue.js(渐进式框架)

## 安装
    npm install vue

## 介绍

- 1.声明式渲染（v-bind）

- 2.条件与循环（v-if  v-for）

- 3.处理用户输入(v-on  v-model)

## Vue 实例

   - 1.创建一个 Vue 实例
    
    
    var vm = new Vue({
      // 选项
    })
    
   - 2.数据与方法
   
   
    // 我们的数据对象
    var data = { a: 1 }
    
    // 该对象被加入到一个 Vue 实例中
    var vm = new Vue({
      data: data //数据属性
    })
    
    // 获得这个实例上的属性
    // 返回源数据中对应的字段
    vm.a == data.a // => true
    
    // 设置属性也会影响到原始数据（Object.freeze()，阻止修改现有的属性）
    vm.a = 2
    data.a // => 2
    
    // ……反之亦然
    data.a = 3
    vm.a // => 3
    
    除数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 $ ，以便与用户定义的属性区分开来
    var data = { a: 1 }
    var vm = new Vue({
      el: '#example',
      data: data
    })
    
    vm.$data === data // => true
    vm.$el === document.getElementById('example') // => true
    
    // $watch 是一个实例方法
    vm.$watch('a', function (newValue, oldValue) {
      // 这个回调将在 `vm.a` 改变后调用
    })
    
   3.声明周期(生命周期钩子的 this 上下文指向调用它的 Vue 实例)
   
   
     beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, beforeDestroy, destroyed
     
## 模板语法
> 1.插值
   
   - 1.1 文本
         数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：
         <span>Message: {{ msg }}</span>
         使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新
         <span v-once>这个将不会改变: {{ msg }}</span>
                 
   - 1.2 原始 HTML（容易导致 XSS 攻击）     
         双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：
         <p>Using mustaches: {{ rawHtml }}</p>
         <p>Using v-html directive: <span v-html="rawHtml"></span></p>
         Using mustaches: <span style="color:red">This should be red.</span>
         Using v-html directive:This should be red.(红色字体)
     
   - 1.3 特性
         Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
         <div v-bind:id="dynamicId"></div>
         
> 2.指令（指令 (Directives) 是带有 v- 前缀的特殊特性）  
   
   - 2.1 参数（一些指令能够接收一个“参数”，在指令名称之后以冒号表示）
         <a v-on:click="doSomething">...</a>  //用于监听 DOM 事件
     
   - 2.2 动态参数（用方括号括起来的 JavaScript 表达式作为一个指令的参数）
         <a v-bind:[attributeName]="url"> ... </a> //attributeName值为“href”, 绑定将等价于 v-bind:href
     
   - 2.3 修饰符（修饰符 (modifier) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定）
         <form v-on:submit.prevent="onSubmit">...</form>  //.prevent 修饰符告诉 v-on 指令对于触发的事件调用 
   
> 3.缩写 

   - v-bind 缩写
         <!-- 完整语法 -->
         <a v-bind:href="url">...</a>
         
         <!-- 缩写 -->
         <a :href="url">...</a>
         
   - v-on 缩写
         <!-- 完整语法 -->
         <a v-on:click="doSomething">...</a>
         
         <!-- 缩写 -->
         <a @click="doSomething">...</a>
         

## 计算属性和侦听器
> 1.计算属性

> 2.侦听器      