## redux异步&middleware

默认情况下，`createStore()` 所创建的Redux Store没有使用middleware，所以只支持同步数据流。

可以使用`applyMiddleware()` 来增强`createStore()` 。虽然这不是必须的，但可以帮助你用简便的方式来描述异步的action。

一般习惯使用`redux-thunk`。通过使用指定的 middleware，action 创建函数除了返回 action 对象外还可以返回函数。

middleware可以让你dispatch一些除了action以外的内容，比如函数或者Promise。所用的middleware以自己的方式解析dispatch的任何内容，并继续传递actions给下一个middleware。

当 middleware 链中的最后一个 middleware 开始 dispatch action 时，这个 action 必须是一个普通对象。



Thunk middleware知道如何处理函数。可以把dispatch方法通过参数的形式传递给函数，以此让函数也能dispatch action。





### middleawre的演进

功能：日志记录和崩溃记录

```
let action = addTodo('Use Redux')
console.log('dispatching', action)
store.dispatch(action)
console.log('next state', store.getState())
```



==> 



```
function dispatchAndLog(store, action){
	console.log('dispatching', action)
	store.dispatch(action)
	console.log('next state', store.getState())
}
dispatchAndLog(store, addTodo('Use Redux'))
```



==>



```
let next = store.dispatch
store.dispatch = function dispatchAndLog(action) {
	console.log('dispatching', action)
	let result = next(action)
	console.log('next state', store.getState())
	return result
}

store.dispatch(addTodo('Use Redux'))
```



==> 添加崩溃报告



```
function patchStoreToAddLogging(store) {
  let next = store.dispatch
  store.dispatch = function dispatchAndLog(action) {
    console.log('dispatching', action)
    let result = next(action)
    console.log('next state', store.getState())
    return result
  }
}

function patchStoreToAddCrashReporting(store) {
  let next = store.dispatch
  store.dispatch = function dispatchAndReportErrors(action) {
    try {
      return next(action)
    } catch (err) {
      console.error('捕获一个异常!', err)
      Raven.captureException(err, {
        extra: {
          action,
          state: store.getState()
        }
      })
      throw err
    }
  }
}

patchStoreToAddLogging(store)
patchStoreToAddCrashReporting(store)

所以这边更新了两次store.dispatch，那就是需要依次执行，得到新的dispatch之后先执行一次。

patchStoreToAddLogging(store)
store.dispatch(addTodo('Use Redux'))
patchStoreToAddCrashReporting(store)
store.dispatch(addTodo('Use Redux'))

但这样不就触发了两次dispatch？不久发起了两次么
```





==>  不替换`store.dispatch`改成函数中返回新的dispatch



```
function logger(store) {
	let next = store.dispatch
	
	
	return function dispatchAndLog(action) {
		console.log('dispatching', action);
		let result = next(action)
		console.log('next state', store.getState())
		return result
	}
}
```

```
function applyMiddlewareByMonkeypatching(store, middlewares) {
  middlewares = middlewares.slice()
  middlewares.reverse()

  // 在每一个 middleware 中变换 dispatch 方法。
  middlewares.forEach(middleware =>
    store.dispatch = middleware(store)
  )
}

所以其实还是更改store.dispatch么，，，
```

```
applyMiddlewareByMonkeypatching(store, [ logger, crashReporter ])
```



==>



```
function logger(store) {
  return function wrapDispatchToAddLogging(next) {
    return function dispatchAndLog(action) {
      console.log('dispatching', action)
      let result = next(action)
      console.log('next state', store.getState())
      return result
    }
  }
}

logger(store)(store.dispatch)(addTodo('Use Redux'))
```

```
const logger = store => next => action => {
  console.log('dispatching', action)
  let result = next(action)
  console.log('next state', store.getState())
  return result
}

const crashReporter = store => next => action => {
  try {
    return next(action)
  } catch (err) {
    console.error('Caught an exception!', err)
    Raven.captureException(err, {
      extra: {
        action,
        state: store.getState()
      }
    })
    throw err
  }
}
```

