## React.Component

每个组件都有一些生命周期方法，你可以通过继承来覆盖

### Mounting

These methods are called in the following order when an instance of component is being created and inserted into the DOM.

下述方法会在一个组件的实例被创建和插入DOM的时候依次执行：

* constructor()
* static getDerivedStateFromProps()
* render()
* componentDidMount()

### Updateing

An update can be caused by changes to props or state. these methods are called in the following order when a component is being re-rendered.

下述方法在组件更新的时候依次执行。

* static getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshopBeforeUpdate()
* componentDidUpdate()



### Unmounting

this method is called when a component is being removed from the DOM

* componentWillUnmount()



### Error Handling

these methods are called when there is an error during rendering, in a lifecycle method, or in the constructor of any child component.

* static getDerivedStateFromError()
* componentDidCatch()



### Other APIs

* setState()
* forceUpdate()





## Common Used Lifecycle Methods

常用的生命周期的方法

### render()

`render` method is only required in a class component.

`render`方法应该是纯粹的，不能修改组件状态。





### constructor()

if you don't initialize state and you don't bind methods, you don't need to implement a constructor for your React component.

如果你不需要设置初始化的state值或绑定事件回调函数，那么不需要调用constructor方法。

Typically, in React constructors are only used for two purposes:

* Initializing local state by assigning an object to this.state
* Binding event handler methods to an instance



Avoiding introducing any side-effects or subscriptions in the constructor. For those use case , use `componentDidMount` instead.



需要避免如下操作：

```
constructor(props){
    super(props);
    this.state = {color: props.color}; // dont't do this
}
```

the problem is that it's both unnecessary (you can use this.props.color directly istead), and create bugs(updates to the color prop won't be reflected in the state)



### componentDidMount()

is invoked immediately after a component is mounted(inserted into the tree). DOM操作和数据记载都应该这边执行。

数据监听之类的也适合在这边处理。



### componentDidUpdate

```
componentDidUpdate(prevProps, prevState, snapshot)
```

is invoked immediately after updating occurs. 在第一次渲染刷新的时候不会调用的。

在这个方法中要小心调用`setState()`方法，不然会进入死循魂的。有个条件判断才是安全的。

如果你在组件中调用了`getSnapshotBeforeUpdate()`方法（很少情况下会调用），那么在`componentDidUpdate()`中，就接受 `snapshot`作为最后一个参数。



### componentWillUnmount

Invoked immediately before a component is unmounted and destroyed. perform any necessary cleanup in this methods, such as invalidating timers, canceling network request, or clean up any subscriptions that called in `componentDidMount`

在该方法中做清理作用，一些计时器、网络请求和订阅都需要在这边清理

you __should not call setState()__ in this method.



## Rarely Used Lifecycle Methods

接下来介绍一些在生命周期中比较少用的方法



### shouldComponentUpdate

```
shouldComponentUpdate(nextPRops, nextState)
```

default to true.

this method only exists as a `performance optimization`。如果值比较简单，尽量用PureComponent。

Not that return `false` does not prevent child components from re-rendering when their state changes.



### static getDerivedStateFromProps()

```
static getDerivedStateFromProps(props, state)
```

`getDerivedStateFromProps` is invoked right before calling the render method, both on the initial mount and on subsequent updates. is should return an object to update the state, or null to update nothing.



### getSnapshotBeforeUpdate()

```
getSnapshotBeforeUpdate(prevProps, prevState)
```

invoked right before the most recently rendered output is committed to the DOM.

Any value returned by this lifecycle will be passed as a parameter to `componentDidUpdate()`



## Error Boundaries

only use error boundaries for recovering from unexcepted exceptions. don't try to use them for control flow.



### static getDerivedStateFromError()

```
static getDerivedStateFromError(error)
```

Invoked after an error has benn thrown by a descendant component. it receives the error that was thrown as a parameter and should return a value to update state.

__getDerivedStateFromError() is called during the "render" phase, so side-effects are not permitted__



### componentDidCatch()

```
componentDidCatch(error, info)
```

`componentDidCatch` is called during the "commit" phase, so side-effects are permitted.



## Other APIs

Methods you can call from your components.

* setState
* forceUpdate



## Class Properties

### defaultProps

can be defined as a property on the component class itself, to set the default props for the class.

used for undefined props ,not for null props.



### displayName

一般用不到，调试的时候可能用下



## Instane Properties

### props

`this.props` contains the props that were defined by the caller of this component.



### state

the state contains data specific to this component that may change over time. The state is user-defined, and it should be a plain JavaScript object.

如果该数据和渲染没有关系，那么可以不放在state里面，可以放对象实例中去。

state中的数据只和渲染有关系。

