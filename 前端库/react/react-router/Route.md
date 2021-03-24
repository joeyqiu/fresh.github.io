## Route

to render some UI when a location matches the route's path.



### Route render methods

There are 3 ways to render something with a `<Router>`:

* `<Route component>`
* `<Route render>`
* `<Route children>`



优先级一次是：Component > render > children



````
if (component) return match ? React.createElement(component, props) : null;

if (render) return match ? render(props) : null;

if (typeof children === "function") return children(props);

if (children && !isEmptyChildren(children)) return React.Children.only(children);
````





### Route props

All three `render methods` will be passed thr same three route props:

* match
* location
* history

