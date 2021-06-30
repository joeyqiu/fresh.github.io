#### 函数中的this

在标准函数中，this引用的是把函数当成方法调用的上下文对象，这时候通常称其为this值（在网页的全局上下文中调用函数时，this指向window）。

这个this值到底引用哪个对象，必须到函数被调用的时候才能确定，因此这个值在代码执行过程中可能会变。



#### 箭头函数中的this

在箭头函数中，this引用的是定义箭头函数的上下文。也可以说this值取决于该函数外部非箭头函数的this值，且不能通过`call(), apply(), bind()` 方法来改变this值。



#### 类中的this

类的声明基本上还是基于自定义类型声明的语法糖，

```javascript
class Person {
	constructor(name) {
		this.name = name;
	}
  getName() {
    return this.name
  }
}

typeof Person : function
```

等价于

```javascript
function Person(name) {
	this.name = name
}
Person.prototype.getName = function() {
  return this.name
}
```

class中的方法，除了静态方法，都是原型链上的，其中的name属性则是自由属性，不会出现在原型上，且只能在类的构造函数或方法中创建。

因为方法是原型上的，所以函数中的this指向的上下文就是实例对象。

```javascript
const joey = new Person('joey')
joey.getName();

//getName中的this，指向的是joey实例。
```



##### 在react中

```javascript
class ButtonDemo extends PureComponent {
  componentDidMount() {
    console.log(this); 实例
    console.log(typeof this); object
    console.log(this instanceof ButtonDemo); true
  }

  hello() {
    console.log(this); // undefined
  }

  render() {
    return (<Button block={true} onClick={this.hello}>Click Me</Button>);
  }
}
```

在react中，hello虽然是ButtonDemo实例的方法，但是调用的上下文是在Button的onClick方法中，这边的上下文指向已经不是ButtonDemo的实例了，最终点击后打印出来的this是undefined。

