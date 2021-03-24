## Code Splitting

Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small chunks which you can then load on demands.

参考网址： 

http://2ality.com/2017/01/import-operator.html#loading-code-on-demand

https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html

https://github.com/jamiebuilds/react-loadable



Create-react-app 可以使用 dynamic import()方法，its proposal is in stage3.  The `import()`函数接受模块名作为参数，然后返回一个`Promise`对象。





example:

#### moduleA.js

```
const moduleA = 'Hello';

export { moduleA };
```

#### App.js

```
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./moduleA')
      .then(({ moduleA }) => {
        // Use moduleA
      })
      .catch(err => {
        // Handle failure
      });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Load</button>
      </div>
    );
  }
}

export default App;
```

This will make `moduleA.js` and all its unique dependencies 



### EMCAScript modules are static

es模块的导入方式是静态的。

You must specify what you import and export at compile time and can't react to changes at runtime.

```
import * as someModule from './dir/someModule.js'
```



### The proposal enables dynamic module imports

草案中的动态载入方法如下：

```
const moduleSpecifier = './dir/someModule.js';
import(moduleSpecifier).then(someModule => someModule.foo());
```

像函数一样使用。



### Use case for import()

#### computed module specifier： 模块名动态生成

```
import(`messages_${getLocale()}.js`)
.then(···);
```



#### Accessing exports via destructuring

```
import('./myModule.js')
.then(({export1, export2}) => {
    ···
});
```

#### dynamically loading multiple modules

```
Promise.all([
    import('./module1.js'),
    import('./module2.js'),
    import('./module3.js'),
])
.then(([module1, module2, module3]) => {
    ···
});
```

#### async functions and import ()

```
async function main() {
    const myModule = await import('./myModule.js');

    const {export1, export2} = await import('./myModule.js');

    const [module1, module2, module3] =
        await Promise.all([
            import('./module1.js'),
            import('./module2.js'),
            import('./module3.js'),
        ]);
}
main();
```

```
(async () => {
    const myModule = await import('./myModule.js');
})();
```





## Code Splitting and React Router v4

常用方式如下：

```
/* Import the components */
import Home from "./containers/Home";
import Posts from "./containers/Posts";
import NotFound from "./containers/NotFound";

/* Use components to define routes */
export default () =>
  <Switch>
    <Route path="/" exact component={Home} />
    <Route path="/posts/:id" exact component={Posts} />
    <Route component={NotFound} />
  </Switch>;
```

这边是把所有需要的组件都加载了，为了实现代码分离，只加载路由对应的组件。

### Create an Async Component

```
import React, { Component } from "react";

export default function asyncComponent(importComponent) {
  class AsyncComponent extends Component {
    constructor(props) {
      super(props);

      this.state = {
        component: null
      };
    }

    async componentDidMount() {
      const { default: component } = await importComponent();

      this.setState({
        component: component
      });
    }

    render() {
      const C = this.state.component;

      return C ? <C {...this.props} /> : null;
    }
  }

  return AsyncComponent;
}
```

We are doing a few things here:

1. The `asyncComponent` function takes an argument; a function (`importComponent`) that when called will dynamically import a given component. This will make more sense below when we use `asyncComponent`.
2. On `componentDidMount`, we simply call the `importComponent` function that is passed in. And save the dynamically loaded component in the state.
3. Finally, we conditionally render the component if it has completed loading. If not we simply render `null`. But instead of rendering `null`, you could render a loading spinner. This would give the user some feedback while a part of your app is still loading.



### Use the Async Component

```
import Home from './containers/Home'
```

===>

```
const asyncHome = asyncComponent(()=> import('./containers/Home'));
```



### Next Steps

如果在动态载入的时候发生错误，可以添加一个提供功能，这边推荐`react-loadable`插件。

```
npm install --save react-loadable
```

然后如下使用

```
const asyncHome = Loadable({
    loader: ()=>import('./containers/Home'),
    loading: MyLoadingComponent
});
```

It’s a simple component that handles all the different edge cases gracefully.

```
const MyLoadingComponent = ({isLoading, error}) => {
  // Handle the loading state
  if (isLoading) {
    return <div>Loading...</div>;
  }
  // Handle the error state
  else if (error) {
    return <div>Sorry, there was a problem loading the page.</div>;
  }
  else {
    return null;
  }
};
```



## Component-based splitting

上面介绍的是基于React路由来进行动态载入，基于路由动态载入的基础上，还能进行一次基于组件的动态载入。使按需加载效果更好。

There are more places that just routes where you can pretty easily split apart your app. Modals, tabs, and many more UI Components hide content util the user has done something to reveal(显示，揭露) it.

除了路由，在app里更多地方可以分离代码。比如弹窗等可以等用户操作之后，需要显示的时候再动态载入。