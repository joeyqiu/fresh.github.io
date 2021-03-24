## Advanced Guides



### Accessibility

是专门为残疾人设置的功能，



### Code-Splitting

代码分割



当使用Babel的时候，需要确保Babel可以解析动态引入(can parse the dynamic import syntax)，而不是转化，因此需要引入babel-plugin-syntax-dynmaic-import.



#### Libraries

* React Loadable
* Route-based code splitting



## Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

Context设计目的主要是为共享那些被认为对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

例如：通过修改主题，所有颜色都被改变，赞







## Error Boundaries

封装渲染时候的错误，很有用

通过`componentDidCatch`来捕获。

```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```



## Forwarding Refs

通过ref获取DOM节点，并把它传递给子节点



```
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```



## Fragments

Fragments let you group a list of children without adding extra nodes to the DOM.

```
 <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
```

简写的方法，直接用空标签，，但是空标签有些地方不支持，所以尽量用`React.Fragment`

```
 <>
      <ChildA />
      <ChildB />
      <ChildC />
    </>

```

Fragment只接受key属性



## Higher-Order Components

高阶组件

a higher-order component is a function that takes a component and returns a new component.

```
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```



### 注意点

#### Don't use HOCs inside the render Method

```
render() {
  // A new version of EnhancedComponent is created on every render
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // That causes the entire subtree to unmount/remount each time!
  return <EnhancedComponent />;
}
```

需要避免在render方法中调用高阶组件，因为每次调用都会返回一个新的组件，这时候React性能优化的算法就没有效果了。



应该在外面创建一次，然后组件内使用



#### Static Methods Must Be Copied Over

因为高阶组件是一个接收组件，并返回新组件的函数，所以在旧组件上定义的静态函数会无效。



#### Refs Aren't Passed Through



## Integrating with Other Libraries

React如何兼容别的js库


### Integrating with DOM Manipulation Plugins

React只会知道内部的DOM操作，如果在React之外的DOM操作，React是不知道的。

The easiest way to avoid conflicts is to prevent the React component from updateing. You can do this by rendering elements that React has no reason to update, like an empty <div />.

最简单的避免冲突的方法就是防止React组件的更新。你可以渲染一个React不需要更新的组件，就像一个空的div一样。



### Integrating with Other View Libbraries

React can be embedded into other applications thanks to the flexibility of `ReactDOM.render()`



It's important that we also call `ReactDOM.unmountComponentAtNode()` in the remove method(反正就是类似解绑清理的函数中) so that React unregisters event handlers and other resources associated with the component tree when it is detached.

React清理的时候，需要调用`ReactDOM.unmountComponentAtNode`方法，在清理的时候需要注意，不清理干净，对性能会有影响。



## JSX In Depth

#### React Must Be in Scope

React必须在页面中引入



#### Using Dot Notation for JSX Type

如果一个模块能够对外提供多个React组件。可以通过`.`来分隔

```
<MyComponents.DatePicker color="blue" />;
```



#### User-Defined Components Must Be Capitalized

jsx中的约定规范：

* 用户自定义的组件必须首字母大写
* 首字母小写的都是内置的



#### Choosing the Type at Runtime

 不能使用表达式作为React元素类型，如果一定要用表达式，可以先赋值到一个首字母大写的变量上去。

```
<components[props.storyType] story={props.story} />;
// Wrong

const SpecificStory = components[props.storyType];
<SpecificStory story={props.story} />;
// Right
```



### Props in JSX

#### JavaScript Expressions as Props

props中可以使用表达式



#### String Literals

给props传入的字符串都会转义处理，所以下面两段代码相等

```
<MyComponent message="&lt;3" />
==
<MyComponent message={'<3'} />
```



#### Props Default to "true"

如果定义的prop没有设置值，默认为`true`.

```
<MyTextBox autocomplete />
==
<MyTextBox autocomplete={true} />
```



#### Spread Attributes

使用es6的扩展属性方法，来传递整个对象

```
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

传递整个对象比较方便，但是会包含一些必须要的属性，最好谨慎使用。



通过用下面的方法，把不想要的单独提取出来。

```
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};
```



### 跳转渲染

貌似下面这种方式好多了

```
{showHeader ? <Header /> : null}

it's better
{showHeader && <Header />}
```



## Optimizing Performance

* Use the Production Build
* Virtualized Long lists(可参考组件：react-window或者react-virtualized)
* Avoid Reconciliation
* shouldComponentUpdate In Action
* Using Immutable Data Structures(immutable.js代码库)



## Portals

Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component

```
ReactDOM.createPortal(child, container)
```



## React Without ES6

```
handleClick = () => {
    alert(this.state.message);
  }
```

通过上述写法，来实现 `this.handleClick=this.handleClick.bind(this)`



## React Without JSX

JSX is not a requirement for using React, Using React without JSX is especially convenient when you don't want to set up compilation in your build environment.



## Reconciliation

React's `diffing` algorithm help component updates are predictable(可预测的) while being fast enough for high-performance apps.



`key`在比较算法中，能有效提高性能。有了`key`之后避免对节点操作过大。

The key only has to be unique among its siblings, not globally unique.

key的话，尽量用列表对象中的id，如果实在是没有id，则使用index也可以，但是如果列表需要重新排序，性能就会比较的差。

you can pass an item's index in the array as a key. This can work well if the items are never reordered, but reorders will be slow.



Component instances are updated and reused based on their key. If the key is an index, moving an item changes it.

组件实例是根据key来更新和复用的，所以如果重新排序，组件的顺序也可能会出错



## Refs and the DOM

Refs provide a way to access DOM nodes or React elements created in the render method.



### Creating Refs

```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```



### Accessing Refs

```
const node = this.myRef.current;
```

ref的值依赖于node的类型：

* 当ref属性被用于html元素上时，ref接收一个DOM元素做个current属性
* 当用于自定义的组件时，接收组件实例作为current属性。
* You may not use the ref attribute on function components because they don't have instance.

不能用于函数式组件上，因为没有实例。



## Render Props

the term `render prop` refers to a technique for sharing code between React components using a prop whose value is a function.

A component with a render prop takes a function that render a React element and calls it instead of implementing its own render logic.

```
<DataProvider render={data => {
    <h1>Hello {data.target}</h1>
}/>
```

__a render prop is a function prop that a component uses to know what to render.__





## Static Type Checking

Static type checkers like `Flow` and `Typescript` identify certain types of problems before you even run your code.通过自动完成功能来提高工作效率, For this reason, we recommend using Flow or Typescript instead of `PropTypes` for larger code base.(大型代码库，推荐用Flow或Typescript而不是PropTypes)



## Strict Mode

`StrictMode` is a tool for highlighting potential problems in an application. Like `Fragment`, `StrictMode` does not render any visible UI. It activates additional checks and warnings for its descendants.

you can enable strict mode for any part of your application. __React.StrictMode__

```
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

`StrictMode` currently helps with:

* Identify components with unsafe lifecycles.
* Warning about legacy string ref API usage
* Detecting unexpected side effects
* Detecting legacy context API



Strict mode can't automatically detect side effects for you, but it can help you spot them by making them a little more deterministic. This is done by intentionally `double-invoking` the following methods:

* Class component `constructor`method
* The `render` method
* `setState`updater  functions(the first argument)
* the static `getDerivedStateFromProps` lifecycle





## Typechecking With PropTypes

React.PropTypes has moved into a different package since React v15.5. Please use the `prop-types`library instead.

For performance reasons. `propTypes` is only checked in development mode.



### Default Prop Values

```
Class Greeting extends Component {
    xxx
}

Greeting.defaultProps = {
    name: 'stranger'
};
```

或者使用类的静态属性

```
class Greeting extends Component {
    static defaultProps = {
        name: 'stranger'
    }
    
    render(){
        xxx
    }
}
```





## Uncontrolled Components

In most case , we recommend using `controlled components` to implement forms. In a controlled component, form data is handled by a React component. The alternative is uncontrolled components, where form data is handled by the DOM itself.

DOM节点自己控制表单数据，React无法控制，就叫uncontrolled，`<input type="text"/>`，如果React可以控制，就会设置value属性，然后通过控制value来控制表单节点。



## Web Components

React and `Web Components` are built to solve different problems. Web Components provide strong encapsulation for reusable components, while React provides a declarative library that keeps the DOM in sync with your data.