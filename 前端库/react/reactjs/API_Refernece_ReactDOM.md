## ReactDOM

the `react-dom` package provides DOM-specific methods that can be used at the top level of your app.

* render()
* hydrate()
* unmountComponentAtNode()
* findDOMNode()
* createPortal()



### render

```
ReactDOM.render(element, container [, callbback])
```

Render a React Component into the DOM in the supplied `container` and return a reference to the component (or return null for stateless components).



### hydrate

```
ReactDOM.hydrate(element, container [, callback])
```

same as render, but is used to hydrate a container whose HTML contents were rendered by `ReactDOMServer`



### unmountComponentAtNode

```
ReactDOM.unmountComponentAtNode(container)
```

Remove a mounted React component from. the DOM and clean up its event handlers and state.



### findDOMNode

```
ReactDOM.findDOMNode(component)
```

if this component has been mounted into the DOM, this returns the corresponding native browser DOM element. __in mose case, you can attach a ref to the DOM node and avoid using findDOMNode at all__



### createPortal

```
ReactDOM.createPortal(child, container)
```

create a portal, Portals provide a way to render children into a DOM node that exists outside the hierarchy of the DOM component.