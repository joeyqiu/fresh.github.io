## React



### React Component

An element describes what you want to see on the screen:

一个React元素描述了你想在屏幕上看到什么。

```
const element = <h1>Hello, world</h1>;
```

React elements are plain objects, and are cheap to create. React DOM takes care of updating the DOM to match the React elements.

React元素是一个纯粹的对象，便于创建。



### Function and Class Components

```
function Welcome(props) {
    return <h1>Hello ,{props.name}</h1>
}

函数组件，接受一个props参数，并返回一个React elemennt。
```

```
class Welcome extends React.Component {
    render(){
        return <h1>Hello ,{props.name}</h1>
    }
}
```

从React视图的观点看来，上面两段代码是相等的。



### props

When React sees an element representing a user-defined component, it passes JSX attributes to this component as a single object. We call this object “props”.

当React看到用户自定义的组件时，它传递的属性被当成一个单对象给过去，被称为props。

React treats components starting with lowercase letters as DOM tags.



### Converting a Function to a Class

You can convert a function component like `Clock` to a class in five steps:

```
1. Create an ES6 class, with the same name, that extends React.Component.

2. Add a single empty method to it called render().

3. Move the body of the function into the render() method.

4. Replace props with this.props in the render() body.

5. Delete the remaining empty function declaration.
```

```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

====>

class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```



### Handling Events

 We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

事件绑定，因为this的问题，推荐用两种方式来实现事件绑定：

* 在构造函数中通过 bind(this)
* public class fields syntax

````
构造函数中bind(this)
constructor(props) {
	super(props);
	this.state = {isToggleOn: true};

	// This binding is necessary to make `this` work in the callback
	this.handleClick = this.handleClick.bind(this);
}
````

````
class xxx extends Component {
  ...
  // This syntax ensures `this` is bound within handleClick.
  handleClick = () => {
    console.log('this is:', this);
  }
}
````





### Lists and Keys

We don’t recommend using indexes for keys if the order of items may change.This can negatively impact performance and may cause issues with component state. 

之所以不推荐使用index作为key，是因为列表中数据项的位置如果会变化的话，会导致负面的性能影响。



## Thinking In React

1. Break the UI Into a Component Hierarchy (根据页面UI去划分组件)
2. Build a Static Version in React(build a version that takes your data model and renders the UI but has no interactivity.根据数据模型和UI，画一个静态的页面)
3. Identify The Minimal(but complete) Representation of UI State
4. Identify Where your State should live
5. Add Inverse Data Flow