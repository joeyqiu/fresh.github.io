## History

* "browser history" : A DOM-specific implementation, useful in web browsers that support the HTML5 history API.
* "hash history": A DOM-specific implmentation for legacy web browsers
* "Memory history": An in-memory history implementation, useful in testing and non-DOM environments like React Native.



`history`对象一般有如下的属性和方法：

* length --  (number) the number of entries in the history stack
* action -- (string) the current action (PUSH, REPLACE, or POP)
* location -- (object) the current location. May have the following properties
  * Pathname -- (string) the path of the url
  * search -- (string) the url query string
  * Hash -- (string) the url hash fragment
  * State -- (object) location-specific state that was provided to e.g. push(path, state) when this location was pushed onto the stack. only available in browser and memory history.
* Push(path, [state]) -- (function) pushes a new entry onto the history stack
* replace(path, [state]) -- (function) replaces the current entry on the history stack
* go(n) -- (function) moves the pointer in the history stack by n entries
* goBack() -- (function) equivalent to go(-1)
* goForward -- (function) equivalent to go(1)
* block(prompt) -- (function) Prevents navigation



### history对象

```
var history = {
    length: globalHistory.length,
    action: 'POP',
    location: initialLocation,
    createHref: createHref,
    push: push,
    replace: replace,
    go: go,
    goBack: goBack,
    goForward: goForward,
    block: block,
    listen: listen
  };
```



### HashHistory---push

源码：

```
var pushHashPath = function pushHashPath(path) {
  return window.location.hash = path;
};

var push = function push(path, state) {
    warning(state === undefined, 'Hash history cannot push state; it is ignored');

    var action = 'PUSH';
    var location = createLocation(path, undefined, undefined, history.location);

    transitionManager.confirmTransitionTo(location, action, getUserConfirmation, function(ok) {
      if (!ok) return;

      var path = createPath(location);
      var encodedPath = encodePath(basename + path);
      var hashChanged = getHashPath() !== encodedPath;

      if (hashChanged) {
      // 如果hash有改动
        // We cannot tell if a hashchange was caused by a PUSH, so we'd
        // rather setState here and ignore the hashchange. The caveat here
        // is that other hash histories in the page will consider it a POP.
        ignorePath = path;
        pushHashPath(encodedPath); // 更新hash值

        var prevIndex = allPaths.lastIndexOf(createPath(history.location));
        var nextPaths = allPaths.slice(0, prevIndex === -1 ? 0 : prevIndex + 1);
        // 从0 开始的nextPaths，其实就是所有的path

        nextPaths.push(path);
        allPaths = nextPaths; //重置所有path

        setState({
          action: action,
          location: location
        }); // 覆盖history对象中对应的字段
      } else {
      // 没有改动，不做处理
        warning(false, 'Hash history cannot PUSH the same path; a new entry will not be added to the history stack');

        setState();
      }
    });
  };
  
setState传入新的location对象和action值，覆盖之前的history对象中对应的字段。

HashHistory的修改，主要就是通过修改hash值来跳转
window.location.hash = path;
```



### HashHistory---replace

```
var replaceHashPath = function replaceHashPath(path) {
  var hashIndex = window.location.href.indexOf('#');

  window.location.replace(window.location.href.slice(0, hashIndex >= 0 ? hashIndex : 0) + '#' + path);
};

var replace = function replace(path, state) {
    warning(state === undefined, 'Hash history cannot replace state; it is ignored');

    var action = 'REPLACE';
    var location = createLocation(path, undefined, undefined, history.location);

    transitionManager.confirmTransitionTo(location, action, getUserConfirmation, function(ok) {
      if (!ok) return;

      var path = createPath(location);
      var encodedPath = encodePath(basename + path);
      var hashChanged = getHashPath() !== encodedPath;

      if (hashChanged) {
        // We cannot tell if a hashchange was caused by a REPLACE, so we'd
        // rather setState here and ignore the hashchange. The caveat here
        // is that other hash histories in the page will consider it a POP.
        ignorePath = path;
        replaceHashPath(encodedPath);
      }

      var prevIndex = allPaths.indexOf(createPath(history.location));

      if (prevIndex !== -1) allPaths[prevIndex] = path;

      setState({
        action: action,
        location: location
      });
    });
  };
  
  
通过window.location.replace来实现
```



### HashHistory---go

```
var go = function go(n) {
  warning(canGoWithoutReload, 'Hash history go(n) causes a full page reload in this browser');

  globalHistory.go(n);
};

globalHistory = window.history;
```



### HashHistory---goBack

```
var goBack = function goBack() {
  return go(-1);
};
```



### HashHistory---goForward

```
var goForward = function goForward() {
  return go(1);
};
```



### BrowserHistory --- Push

```
globalHistory.pushState({ key: key, state: state }, null, href);

var push = function push(path, state) {
    warning(!((typeof path === 'undefined' ? 'undefined' : _typeof(path)) === 'object' && path.state !== undefined && state !== undefined), 'You should avoid providing a 2nd state argument to push when the 1st ' + 'argument is a location-like object that already has state; it is ignored');

    var action = 'PUSH';
    var location = createLocation(path, state, createKey(), history.location);

    transitionManager.confirmTransitionTo(location, action, getUserConfirmation, function (ok) {
      if (!ok) return;

      var href = createHref(location);
      var key = location.key,
          state = location.state;


      if (canUseHistory) {
        globalHistory.pushState({ key: key, state: state }, null, href);

        if (forceRefresh) {
          window.location.href = href;
        } else {
          var prevIndex = allKeys.indexOf(history.location.key);
          var nextKeys = allKeys.slice(0, prevIndex === -1 ? 0 : prevIndex + 1);

          nextKeys.push(location.key);
          allKeys = nextKeys;

          setState({ action: action, location: location });
        }
      } else {
        warning(state === undefined, 'Browser history cannot push state in browsers that do not support HTML5 history');

        window.location.href = href;
      }
    });
  };
```

其实BrowserHistory的修改，其实就是通过H5的history对象的pushState方法来修改url，如果不支持History对象，则直接通过`location.href`来更新整个url





### BrowserHistory --- replace

```
globalHistory.replaceState({ key: key, state: state }, null, href);

var replace = function replace(path, state) {
    warning(!((typeof path === 'undefined' ? 'undefined' : _typeof(path)) === 'object' && path.state !== undefined && state !== undefined), 'You should avoid providing a 2nd state argument to replace when the 1st ' + 'argument is a location-like object that already has state; it is ignored');

    var action = 'REPLACE';
    var location = createLocation(path, state, createKey(), history.location);

    transitionManager.confirmTransitionTo(location, action, getUserConfirmation, function (ok) {
      if (!ok) return;

      var href = createHref(location);
      var key = location.key,
          state = location.state;


      if (canUseHistory) {
        globalHistory.replaceState({ key: key, state: state }, null, href);

        if (forceRefresh) {
          window.location.replace(href);
        } else {
          var prevIndex = allKeys.indexOf(history.location.key);

          if (prevIndex !== -1) allKeys[prevIndex] = location.key;

          setState({ action: action, location: location });
        }
      } else {
        warning(state === undefined, 'Browser history cannot replace state in browsers that do not support HTML5 history');

        window.location.replace(href);
      }
    });
  };
```

通过history对象的replaceState来实现更新。