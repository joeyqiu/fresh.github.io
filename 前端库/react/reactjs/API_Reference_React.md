# API Reference



## React

### React.component

is the base class for React components when they are defined using ES6 classes.



### React.PureComponent

React.Component doesn't implement `shouldComponentUpdate()`, but React.pureComponent implements it with a shallow prop and state comparison.



如果你组件中的props和state数据结构比较简单，而且不咋会修改的话，可以继承PureComponent做一个 性能优化。

如果是复杂的数据结构，PureComponent是处理不了的，或者可以用immutable的数据。

父组件不刷新会导致里面的子组件也不刷新，最好确定里面的子组件也是pure的。



### createElement()

```
React.createElement(type, [props], [...children])
```

Create and return a new `React element` of the given type.



### cloneElement()

```
React.cloneElement(element, [props], [...children])
```

Clone and return a new React element using `element` as the starting point.

`key`and `ref`会保留原来元素的值。



`React.cloneElement()` is almost equivalent to :

```
<element.type {...element.props} {...props}>{children}</element.type>
```



### createFactory()

```
React.createFactory(type)
```

Return a function that produces React elements of a given type. 不建议使用，推荐使用JSX或者`createElement`代替。



### isValidElement()

```
React.isValidElement(object)
```

Verifies the object is a React element. Return `true` or `false`





### React.Children

提供方法来处理`this.props.children`数据结构。

#### React.Children.map

```
React.Children.map(children, function[(thisArg)])
```

if children is null or undefined, this method will return null or undefined rather than an array.



#### React.Children.forEach

```
React.Children.forEach(children, function[(thisArg)])
```

只是遍历，不返回数组



#### React.children.count

```
React.Children.count(children)
```

return the total number of components in children



#### React.Children.only

```
React.Children.only(children)
```

验证children只有一个child且返回，否则返回错误.



#### React.Children.toArray

````
React.Children.toArray(children)
````



### React.Fragment

let you return multiple elements in a `render()` method without creating an additional DOM element.



### React.createRef

creates a `ref` that can be attached to React elements via the ref attribute.



### React.forwardRef

Create a React Component that forwards the `ref` attribute it receives to another component below in the tree.