## API

API for React Router.

## BrowserRouter

A `<Router>`that uses the HTML5 history API(pushState, replaceState and the popState event) to keep your UI in sync with the URL.



````
import { BrowserRouter } from 'react-router-dom'

<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App/>
</BrowserRouter>
````

* basename(String): the base URL for all locations.
* getUserConfirmation(func): A function to use to confirm navigation(但是试用下来没有触发)
* forceRefresh(bool): if true the router will use full page refreshes on page navigation.
* keyLength(Number): the length of location.key, default to 6.
* children(node): A single child element to render



## HashRouter

A `Router` that uses the hash portion of the URL(i.e. window.location.hash) to keep your UI in sync with the URL.

Hash不支持 `location.key or location.state`，，因此建议使用`BrowserHistory`



可接受的属性和上面类似: `basename, getUserConfirmation, hasType, children`



### hashType

the type of encoding to use for `window.location.hash`

* slash:  创建的hash如：`#/` and `#/sunshine/lollipops`
* noslash: `#` and `#sunshine/lollipops`
* hashbang:  create `ajax crawlable(不被google推荐)` , `#!/` and `#!/sunshine/lollipops`

default to `slash`



## Link

在你的应用中提供一个声明的，可访问的导航。

可以接受的属性：

* to (string/object): 目的地址，可以是字符串或对象
* replace(bool): true的时候，在历史记录中取代当前的入口，而不是添加一个新的
* innerRef(function): allows access to the underlying ref of the component
* others: props you'd like to be on the `<a>` such as title, id...





## MemoryRouter

a `<Router>`that keeps the history of your URL In memory(does not read or write to the website bar) . Useful in tests and non-browser environments like `React Native`



## Redirect

Rendering a `<Redirect>`will navigate to a new location, The new location will override the current location in the history stack, like server-side redirects (http 3xxx) do.



## Route

Its most basic responsibility is to render some UI when a `location` matches the route's path.