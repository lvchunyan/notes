## Vuex(状态管理模式)

- 每一个 Vuex 应用的核心就是 **store（仓库）**。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)。Vuex 和单纯的全局对象有以下两点不同：

 1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

 2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是**显式地提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。
 
    
    // 如果在模块化构建系统中，请确保在开头调用了 Vue.use(Vuex)
    const store = new Vuex.Store({
      state: {
        count: 0
      },
      mutations: {
        increment (state) {
          state.count++
        }
      }
    })
    
    通过 store.state 来获取状态对象，以及通过 store.commit 方法触发状态变更：
    store.commit('increment')
    console.log(store.state.count) // -> 1
    
## State
- **mapState 辅助函数(一个组件获取多个状态)**

    
    // 在单独构建的版本中辅助函数为 Vuex.mapState
    import { mapState } from 'vuex'
    
    export default {
      // ...
      computed: mapState({
        // 箭头函数可使代码更简练
        count: state => state.count,
    
        // 传字符串参数 'count' 等同于 `state => state.count`
        countAlias: 'count',
    
        // 为了能够使用 `this` 获取局部状态，必须使用常规函数
        countPlusLocalState (state) {
          return state.count + this.localCount
        }
      })
    }
   
- **当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组**
    
    
    computed: mapState([
      // 映射 this.count 为 store.state.count
      'count'
    ])
    
- **对象展开运算符**

    
    mapState 函数返回的是一个对象
    
    computed: {
      localComputed () { /* ... */ },
      // 使用对象展开运算符将此对象混入到外部对象中
      ...mapState({
        // ...
      })
    }
    
## getter（store 的计算属性）
- **像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算**


    const store = new Vuex.Store({
         state: {
           todos: [
             { id: 1, text: '...', done: true },
             { id: 2, text: '...', done: false }
           ]
         },
         getters: {
           doneTodos: state => {
             return state.todos.filter(todo => todo.done)
           }
         }
    })
    
- **通过属性访问（getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的）**


    Getter 会暴露为 store.getters 对象，你可以以属性的形式访问这些值：
    store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
    
    Getter 也可以接受其他 getter 作为第二个参数：
    getters: {
      // ...
      doneTodosCount: (state, getters) => {
        return getters.doneTodos.length
      }
    }
    
    结果：
    computed: {
      doneTodosCount () {
        return this.$store.getters.doneTodosCount  // -> 1
      }
    }
    
- **通过方法访问（getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果）**


    通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组进行查询时非常有用。
    getters: {
           // ...
           getTodoById: (state) => (id) => {
             return state.todos.find(todo => todo.id === id)
           }
    }
    
    store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
    
- **mapGetters 辅助函数**  


    mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：
    import { mapGetters } from 'vuex'
    
    export default {
      // ...
      computed: {
      // 使用对象展开运算符将 getter 混入 computed 对象中
        ...mapGetters([
          'doneTodosCount',
          'anotherGetter',
          // ...
        ])
      }
    }
    
    如果你想将一个 getter 属性另取一个名字，使用对象形式：
    mapGetters({
      // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
      doneCount: 'doneTodosCount'
    })
    
## mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 **一个 回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

    const store = new Vuex.Store({
      state: {
        count: 1
      },
      mutations: {
        increment (state) {
          // 变更状态
          state.count++
        }
      }
    })
你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 increment 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 store.commit 方法：

    store.commit('increment')
- **提交载荷（Payload）**

    
    向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：
    // ...
    mutations: {
      increment (state, n) {
        state.count += n
      }
    }
    
    store.commit('increment', 10)
    
    在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：
    // ...
    mutations: {
      increment (state, payload) {
        state.count += payload.amount
      }
    }
    
    store.commit('increment', {
      amount: 10
    })
    
- **对象风格的提交方式**
    
    
    提交 mutation 的另一种方式是直接使用包含 type 属性的对象：
    store.commit({
      type: 'increment',
      amount: 10
    })
    当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：
    mutations: {
      increment (state, payload) {
        state.count += payload.amount
      }
    }
    
- **Mutation 必须是同步函数**

因为当 mutation 触发的时候，回调函数还没有被调用,实际上是不知道什么时候回调函数被调用

- **mapMutations 辅助函数**  

在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）
  
  
    import { mapMutations } from 'vuex'
      
      export default {
        // ...
        methods: {
          ...mapMutations([
            'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`
      
            // `mapMutations` 也支持载荷：
            'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
          ]),
          ...mapMutations({
            add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
          })
        }
      }

## action
- **Action 类似于 mutation，不同在于：**
 1. Action 提交的是 mutation，而不是直接变更状态。
 2. Action 可以包含任意异步操作。
 
 
    const store = new Vuex.Store({
        state: {
          count: 0
        },
        mutations: {
          increment (state) {
            state.count++
          }
        },
        actions: {
          increment (context) {
            context.commit('increment')
          }
        }
        //actions: {
        //      increment ({ commit }) {
        //        commit('increment')
        //      }
        //}
    })
    Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters

- **分发 Action**
    
    
    Action 通过 store.dispatch 方法触发：
    store.dispatch('increment') 
action **内部执行异步操作**

    actions: {
      incrementAsync ({ commit }) {
        setTimeout(() => {
          commit('increment')
        }, 1000)
      }
    }
    Actions 支持同样的载荷方式和对象方式进行分发：
    // 以载荷形式分发
    store.dispatch('incrementAsync', {
      amount: 10
    })
    
    // 以对象形式分发
    store.dispatch({
      type: 'incrementAsync',
      amount: 10
    })
    
- **mapActions 辅助函数**

在组件中使用 this.$store.dispatch('xxx') 分发 action，或者使用 mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用（需要先在根节点注入 store）：
  
    
    import { mapActions } from 'vuex'
    
    export default {
      // ...
      methods: {
        ...mapActions([
          'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`
    
          // `mapActions` 也支持载荷：
          'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
        ]),
        ...mapActions({
          add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
        })
      }
    }

## modules
将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：
    
    
    const moduleA = {
      state: { ... },
      mutations: { ... },
      actions: { ... },
      getters: { ... }
    }
    
    const moduleB = {
      state: { ... },
      mutations: { ... },
      actions: { ... }
    }
    
    const store = new Vuex.Store({
      modules: {
        a: moduleA,
        b: moduleB
      }
    })
    
    store.state.a // -> moduleA 的状态
    store.state.b // -> moduleB 的状态

## 项目结构

- Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

 1. 应用层级的状态应该集中到单个 store 对象中。

 2. 提交 mutation 是更改状态的唯一方法，并且这个过程是同步的。

 3. 异步逻辑都应该封装到 action 里面。

- 如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

     
     ├── index.html
     ├── main.js
     ├── api
     │   └── ... # 抽取出API请求
     ├── components
     │   ├── App.vue
     │   └── ...
     └── store
         ├── index.js          # 我们组装模块并导出 store 的地方
         ├── actions.js        # 根级别的 action
         ├── mutations.js      # 根级别的 mutation
         └── modules
             ├── cart.js       # 购物车模块
             └── products.js   # 产品模块