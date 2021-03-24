## redux思路

随着单页应用的日趋复杂， 页面中管理着比任何时候都多的state（状态）。redux试图让state的变化变得可预测。



### redux三大原则

* 单一数据源

整个应用的state被存储在一棵object tree中，并且这个object tree只存在于唯一一个store中。

* state是只读的

唯一改变state的方法就是触发action，action是一个用于描述已发生事件的普通对象。

* 使用纯函数来执行修改

为了描述action如何改变state tree，你需要编写reducers。





### 生态系统

是一个混合产物，所以可以用各种组建来搭配实现，选择自己熟悉的就好





### Action

Action是把数据从应用传到store的有效载荷。它是store数据的唯一来源。一般来说会通过`store.dispatch()` 将action传到store。

本质上是一个普通对象，约定action内必须使用一个字符串类型的`type` 字段来表示将要执行的动作。



一个添加任务的action，action默认定义成字符串常量。

```
const ADD_TODO = 'ADD_TODO'

{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```





### Reducer

reducers指定了应用状态的变化如何响应actions并发送到store的。

随着reducer的庞大，可以把reducer进行拆分，组成reducers。然后每个reducer只负责管理全局state中它负责的一部分。每个reducer的state参数都不同，分别对应它管理着的那部分state数据。

通过redux提供的`combineReducers` 函数来拆分组成



```
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

等价于下面的写法

```
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

visibilityFilter和todos是两个reducer，分别处理根state中的不同数据，，，这样就把根state拆分成了更简单易更新的方式。



### Store

store是把action和reducer联系到一起的对象。有以下职责

- 维持应用的 state；
- 提供 `getState()` 方法获取 state；
- 提供 `dispatch(action)` 方法更新 state；
- 通过 `subscribe(listener)` 注册监听器;
- 通过 `subscribe(listener)` 返回的函数注销监听器。

redux应用只有一个单一的store。





### 数据流

严格的单项数据流是Redux架构的设计核心。

Redux应用中数据的生命周期遵循下面4个步骤：

* 调用`store.dispatch(action)`
* Redux store调用传入的reducer函数

Store会把两个参数传入reducer：当前的state树和action。

* 根reducer应该把多个子reducer输出合并成一个单一的state树。

根 reducer 的结构完全由你决定。Redux 原生提供`combineReducers()`辅助函数，来把根 reducer 拆分成多个函数，用于分别处理 state 树的一个分支。

* Redux store保存了根reducer返回的完整state树。

















