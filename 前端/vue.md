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

> 1.计算属性(复杂逻辑，computed)

    <div id="example">
      <p>Original message: "{{ message }}"</p>
      <p>Computed reversed message: "{{ reversedMessage }}"</p>
    </div>
    var vm = new Vue({
      el: '#example',
      data: {
        message: 'Hello'
      },
      computed: {
        // 计算属性的 getter
        reversedMessage: function () {
          // `this` 指向 vm 实例
          return this.message.split('').reverse().join('')  //翻转字符串
        }
      }
    })
    结果：
    Original message: "Hello"
    Computed reversed message: "olleH"
    
    另外可以通过方法来实现上述问题：
    <p>Reversed message: "{{ reversedMessage() }}"</p>
    // 在组件中
    methods: {
      reversedMessage: function () {
        return this.message.split('').reverse().join('')
      }
    }
    
    * 注：计算属性缓存 vs 方法的不同是计算属性是基于它们的依赖进行缓存的
    
    计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
    
    computed: {
      fullName: {
        // getter
        get: function () {
          return this.firstName + ' ' + this.lastName
        },
        // setter
        set: function (newValue) {
          var names = newValue.split(' ')
          this.firstName = names[0]
          this.lastName = names[names.length - 1]
        }
      }
    }
    现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。
> 2.侦听器（watch）

## Class 与 Style 绑定
> 绑定 HTML Class（区分在于逗号和冒号之别）

- 对象语法（传给 v-bind:class 一个对象，以动态地切换 class）
      <div v-bind:class="{ active: isActive }"></div>
      // isActive：true; 渲染：<div class="active"></div>
      // isActive：false; 渲染：<div></div>
      也可以在对象中传入更多属性来动态切换多个 class
      <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
      // isActive：true, hasError：false; 渲染：<div class="static active"></div>
- 数组语法（把一个数组传给 v-bind:class，以应用一个 class 列表）
      <div v-bind:class="[activeClass, errorClass]"></div>
      //activeClass: 'active',errorClass: 'text-danger'; 渲染：<div class="active text-danger"></div>
      如果你也想根据条件切换列表中的 class，可以用三元表达式：
      <div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
      // isActive:true; 渲染：<div class="activeClass errorClass"></div>
      // isActive:false; 渲染：<div class="errorClass"></div>
- 用在组件上
      组件：Vue.component('my-component', {
                 template: '<p class="foo bar">Hi</p>'
               })
      加 class: <my-component class="baz boo"></my-component>
      渲染：<p class="foo bar baz boo">Hi</p>
      带数据绑定 class ：<my-component v-bind:class="{ active: isActive }"></my-component>
      渲染：<p class="foo bar active">Hi</p>
> 绑定内联样式

- 对象语法

  v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名
      <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div> //activeColor: 'red', fontSize: 30
      <div v-bind:style="styleObject"></div>  //直接绑定到一个样式对象
      data: {
        styleObject: {
          color: 'red',
          fontSize: '13px'
        }
      }
      
- 数组语法
  
  v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上：
       <div v-bind:style="[baseStyles, overridingStyles]"></div>
- 自动添加前缀

      -webkit- (谷歌, Safari, 新版Opera浏览器等)
      -moz- (火狐浏览器)
      -o- (旧版Opera浏览器等)
      -ms- (IE浏览器 和 Edge浏览器)
## 条件渲染
> v-if v-else-if v-else

      <h1 v-if="awesome">Vue is awesome!</h1>
      <h1 v-else>Oh no 😢</h1>
      
      key 管理可复用的元素
      <template v-if="loginType === 'username'">
        <label>Username</label>
        <input placeholder="Enter your username" key="username-input">
      </template>
      <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address" key="email-input">
      </template>
      * 每次切换时，输入框都将被重新渲染(即清空上次输入的内容)
      * 注意，<label> 元素仍然会被高效地复用，因为它们没有添加 key 属性。
> v-show 

      不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS 属性 display。
      * 注：v-show 不支持 <template> 元素，也不支持 v-else。
> v-if vs v-show

      v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
      v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
      相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
      一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
> v-if 与 v-for 一起使用
     
      * 注：不推荐同时使用 v-if 和 v-for
      当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级