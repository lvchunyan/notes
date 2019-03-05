## vue.js(渐进式框架)

> 安装：npm install vue

> 内容

- 1.声明式渲染（v-bind）
- 2.条件与循环（v-if  v-for）
- 3.处理用户输入(v-on  v-model)
- 4.Vue 实例
   - 4.1 创建一个 Vue 实例
    
    
    var vm = new Vue({
      // 选项
    })
    
   - 4.2 数据与方法
   
   
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
    
   - 4.3 声明周期