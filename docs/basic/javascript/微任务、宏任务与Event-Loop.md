参考文档：https://juejin.im/post/6844903657264136200

什么是宏任务和微任务 由于Javascript是单线程的脚本语言，如果所有代码都是同步执行，当遇到耗时操作时，就会使得浏览器进入假死状态，因此异步任务应运而生。 当执行同步任务遇到一个异步任务时，就在event table（事件表）中注册回调函数，同步任务继续执行。期间异步任务完成时，回调函数会被放入event queue（事件队列/任务队列）。

任务队列中都是已经完成的异步操作，而不是说注册一个异步任务就会被放在这个任务队列中。 **在当前的微任务没有执行完成时，是不会执行下一个宏任务的。**

异步任务之间也是有区别的。宿主环境（浏览器、node）提供的方法是宏任务，例如setTimeout, setInterval。语言标准（js引擎）提供的是微任务，例如Promise。

我们可以从一道经典面试题体验一下事件循环，以下代码会输出什么呢？

```
setTimeout(_ => console.log(4)) 
new Promise(resolve => { 
   resolve()
   console.log(1)
}).then(_ => {
   console.log(3)
})
console.log(2)
```

在这个例子中整体代码和setTimeout是宏任务，Promise.then是微任务。

所有会进入的异步都是指的事件回调中的那部分代码。

也就是说new Promise在实例化的过程中所执行的代码都是同步进行的，而then中注册的回调才是异步执行的。 在同步代码执行完成后才回去检查是否有异步任务完成，并执行对应的回调，而微任务又会在宏任务之前执行。 所以就得到了上述的输出结论1、2、3、4。

### 宏任务

| #                     | 浏览器 | Node |
| --------------------- | ------ | ---- |
| I/O                   | ✅      | ✅    |
| setTimeout            | ✅      | ✅    |
| setInterval           | ✅      | ✅    |
| setImmediate          | ❌      | ✅    |
| requestAnimationFrame | ✅      | ❌    |

### 微任务

| #                          | 浏览器 | Node |
| -------------------------- | ------ | ---- |
| process.nextTick           | ❌      | ✅    |
| MutationObserver           | ✅      | ❌    |
| Promise.then catch finally | ✅      | ✅    |

### EventLoop

当同步任务执行完成，依次执行微任务队列中的所有微任务。执行完所有微任务后，从宏任务队列中获取新的宏任务执行。这样就完成了一个事件循环。 ![事件循环](https://img11.360buyimg.com/imagetools/jfs/t1/94606/28/16238/27262/5e79d161E9acacc42/94bbbd8dd741825e.jpg?ynotemdtimestamp=1626536371606) 事件循环