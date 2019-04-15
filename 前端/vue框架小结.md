## vue

- **MVVM模式(Model-View-ViewModel)--模型-视图-视图模型**

- **View和ViewModel之间通过双向绑定建立联系。**

- **vue.js核心功能：数据的双向绑定（数据和视图的双向绑定）**

- **v-show不能在template上使用**

- **props(传递数据)、events（触发事件）和slot(内容分发)构成Vue组件的3个API来源**

- **Vue中DOM更新是异步的**

_____

## vuex

- **Vuex总共有5个选项：state, mutations, getters, actions, modules**

- **在组件中，来自store的数据只能读取，不能手动修改，改变store中数据的唯一途径就是显式的提交(commit)mutations。**

- **mutations是Vuex的第二选项，用来直接修改state里面的数据；在组件中，通过this.$store.commit('mutations中的方法', 参数)方法执行mutations**

- **组件只负责提交一个事件名，Vuex对应的mutations来完成业务逻辑**

- **mutations里尽量不要异步操作数据。如果异步操作数据了，组件在commit后，数据不能立即改变，而且不知道什么时候会改变**

- **actions里提交的是mutations，并且可以一步操作业务逻辑。actions在组件内通过$store.dispatch触发**

- **mutations、actions很相似，涉及改变数据的使用mutations,存在业务逻辑的使用actions**

- **modules将store分割到不同的模块。当store里的state、getters、mutations、actions很多，放到main.js里面显示很不友好，使用modules可以把她们写到不同的文件中。每个modules拥有自己的state、getters、mutations、actions，而且可以嵌套**
_____

## webpack