## vue-router(路由管理器)

> 安装：npm install vue-router

> 配置：
       * import Vue from 'vue'
       * import VueRouter from 'vue-router'
       * Vue.use(VueRouter)
 
> 内容：在Vue实例内部，你可以通过$router访问路由实例。因此你可以调用this.$router.push

>> router.push (向history栈添加一个新的记录，可以返回之前的URL)
      
| 声明式 | 编程式 |
| ------ | ------ |
| router-link | router.push(...) |

       router.push('home')  // 字符串
       router.push({ path: 'home' })  // 对象
       router.push({ name: 'user', params: { userId: '123' }})  // 命名的路由
       router.push({ path: 'register', query: { plan: 'private' }})  // 带查询参数，变成 /register?plan=private
       
       const userId = '123'
       router.push({ name: 'user', params: { userId }}) // -> /user/123
       router.push({ path: `/user/${userId}` }) // -> /user/123
       // 这里的 params 不生效
       router.push({ path: '/user', params: { userId }}) // -> /user

>> router.replace(替换当前history栈中的记录)

| 声明式 | 编程式 |
| ------ | ------ |
| router-link replace | router.replace(...) |

>> router.go(n)(参数是一个整数，意思是在历史记录中向前或者后退多少步)

       router.go(1)  // 在浏览器记录中前进一步，等同于 history.forward()
       router.go(-1)  // 后退一步记录，等同于 history.back()
       router.go(3)  // 前进 3 步记录
       router.go(-100)  // 如果 history 记录不够用，那就默默地失败呗
       router.go(100)  // 如果 history 记录不够用，那就默默地失败呗
       
>> router-view(命名视图：一个视图使用一个组件渲染)

>> 导航守卫（主要用来通过跳转或取消的方式守卫导航）

   * 全局前置守卫（router.beforeEach）
   
   
       const router = new VueRouter({ ... })
       router.beforeEach((to, from, next) => {
             // ...
       })
       当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫解决完之前一直处于等待中。
       每个守卫方法接收三个参数：
       to: Route：即将要进入的目标路由对象
       from: Route：当前导航正要离开的路由
       next: Function：一定要调用该方法来解决这个钩子。执行效果依赖next方法的调用参数。
           next()：进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是确认（确认的）。
           next(false)：中断当前的导航。如果浏览器的URL改变了（可能是用户手动或者浏览器后退按钮），那么URL地址会重置到from路由对应的地址。
           next('/')或者next({ path: '/' })：跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向next传递任意位置对象，且允许设置诸如replace: true，name: 'home'之类的选项以及任何用在router-link的toprop或router.push中的选项。
           next(error)：（2.4.0+）如果传入next的参数是一个Error实例，则导航会被终止且该错误会被传递给router.onError()注册过的回调。
       确保要调用next方法，否则钩子就不会被解决。

   * 全局解析守卫(router.beforeResolve)
      
      
       同router.beforeEach类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用
    
   * 全局后置钩子（router.afterEach） 
   
   
       和守卫不同的是，钩子这些不会接受next函数，不会改变导航本身
       
   * 路由独享的守卫（beforeEnter）
    
    
       在路由配置上直接定义beforeEnter守卫（与全局前置守卫的方法参数是一样的）：
       const router = new VueRouter({
         routes: [
           {
             path: '/foo',
             component: Foo,
             beforeEnter: (to, from, next) => {
               // ...
             }
           }
         ]
       })
       
   * 组件内的守卫
   
       
       beforeRouteEnter
       beforeRouteUpdate （2.2新增）
       beforeRouteLeave
       
       const Foo = {
         template: `...`,
         beforeRouteEnter (to, from, next) {
           // 在渲染该组件的对应路由被 confirm 前调用
           // 不！能！获取组件实例 `this`
           // 因为当守卫执行前，组件实例还没被创建
         },
         beforeRouteUpdate (to, from, next) {
           // 在当前路由改变，但是该组件被复用时调用
           // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
           // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
           // 可以访问组件实例 `this`
         },
         beforeRouteLeave (to, from, next) {
           // 导航离开该组件的对应路由时调用
           // 可以访问组件实例 `this`
         }
       }
       
       beforeRouteEnter (to, from, next) {
         next(vm => {
           // 通过 `vm` 访问组件实例（this）
         })
       }
       beforeRouteUpdate (to, from, next) {
         // just use `this`
         this.name = to.params.name
         next()
       }
       beforeRouteLeave (to, from , next) {
         const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
         if (answer) {
           next()
         } else {
           next(false)
         }
       }
       
       