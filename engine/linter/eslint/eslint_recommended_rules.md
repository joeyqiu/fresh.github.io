## eslint recommended rules

https://eslint.org/docs/rules/

这边是eslint官方的一些规则。



### Possible Errors

可能的语法和逻辑错误

| 规则名称                                                | 规则说明                                                     |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| [for-direction](#for-direction)                         | enforce "for" loop update clause moving the counter in the right direction. |
| [getter-return](#getter-return)                         | enforce 'return' statements in getters                       |
| [no-async-promise-executor](#no-async-promise-executor) | disallow using an async function as a Promise executor       |
| [no-await-in-loop](#no-await-in-loop)                   | disallow 'await' inside of loops                             |



#### for-direction

A `for` loop with a stop condition that can never be reached, such as one with a counter that moves in the wrong direction, will run infinitely. While there are occasions when an infinite loop is intended, the convention is to construct such loops as `while` loops. More typically, an infinite for loop is a bug.

如果for循环的条件永远不会满足，就会无限执行下去，说明判断条件的方向有错误。无限执行判断适合使用while语句。

##### 错误demo

```
/*eslint for-direction: "error"*/
for (var i = 0; i < 10; i--) {}
for (var i = 10; i >= 0; i++) {}
// 两个都是无限执行
```

##### 正确demo

```
for (var i = 0; i < 10; i++) {}
// 可以执行结束
```



---



#### getter-return

The get syntax binds an object property to a function that will be called when that property is looked up. It was first introduced in ECMAScript 5:

当访问一个对象的属性时，属性绑定的get函数就会被调用。每个get函数都最好有个返回值。

```
var p = {
        get name(){
            return "nicholas";
        }
    };

    Object.defineProperty(p, "age", {
        get: function (){
            return 17;
        }
    });
```

##### 错误的demo

````
p = {
    get name(){
        // no returns.
    }
};

Object.defineProperty(p, "age", {
    get: function (){
        // no returns.
    }
});

class P{
    get name(){
        // no returns.
    }
}
````

##### 正确的demo

````
p = {
    get name(){
        return "nicholas";
    }
};

Object.defineProperty(p, "age", {
    get: function (){
        return 18;
    }
});

class P{
    get name(){
        return "nicholas";
    }
}
````

##### Options

* "allowImplicit": false, 允许return返回一个undefined值。就直接return

```
/*eslint getter-return: ["error", { allowImplicit: true }]*/
p = {
    get name(){
        return; // return undefined implicitly.
    }
};
```



---



#### no-async-promise-executor

不允许使用async函数作为一个Promise的执行函数。

Promise接收一个执行函数作为参数，这个函数有resolve和reject两个参数用来控制promise的状态。

```
const result = new Promise(function executor(resolve, reject) {
  readFile('foo.txt', function(err, result) {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});
```

当执行函数是一个async函数时，是一种错误的使用方式，下述理由会解释：

* 如果一个async执行函数抛出一个错误，这个错误会丢失，并且并不会新创建的Promise构造函数所捕获来reject。这是难以调试且会造成错误的。
* 如果一个promise执行函数使用了await，就意味着没有必要使用Promise了。

##### 错误demo

```
const foo = new Promise(async (resolve, reject) => {
  readFile('foo.txt', function(err, result) {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const result = new Promise(async (resolve, reject) => {
  resolve(await foo);
});
```

##### 正确demo

````
const foo = new Promise((resolve, reject) => {
  readFile('foo.txt', function(err, result) {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

const result = Promise.resolve(foo);
````



---



#### no-await-in-loop

遍历执行是一个常见的操作。但是，在遍历中执行每次都执行`await`操作，意味着并没有充分利用好`async/await` 的并行优势。

通常这样的代码就需要被重构成使用promise了，并且通过`Promise.all()`函数来获取结果。否则的话，每次执行await，都会阻塞到等单次遍历执行完成之后再执行下一次。

##### 错误demo

```
async function foo(things) {
  const results = [];
  for (const thing of things) {
    // Bad: each loop iteration is delayed until the entire asynchronous operation completes
    results.push(await bar(thing));
  }
  return baz(results);
}
```

需要被重构为

##### 正确demo

```
async function foo(things) {
  const results = [];
  for (const thing of things) {
    // Good: all asynchronous operations are immediately started.
    results.push(bar(thing));
  }
  // Now that all the asynchronous operations are running, here we wait until they all complete.
  return baz(await Promise.all(results));
}
```

##### When Not To Use It

有些情况下，是不建议使用该规则的，就会建议在遍历中使用await。所以需要具体代码具体分析，并不是固定死的。

比如一些遍历循环，但是每次遍历并不是完全独立的，

* 可能一次遍历的输出会需要作为下一次遍历的输入，
* 或者在失败的时候，需要进行重复执行
*  loops may be used to prevent your code from sending an excessive amount of requests in parallel.



---







