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
| [no-compare-neg-zero](#no-compare-neg-zero)             | disallow comparing against -0                                |
| [no-cond-assign](#no-cond-assign)                       | disallow assignment operators in conditional expressions     |
| [no-console](#no-console)                               | disallow the use of `console`                                |
| [no-constant-condition](#no-constant-condition)         | Disallow constant expressions in conditions                  |
| [no-control-regex](#no-control-regex)                   | Disallow control characters in regular expressions           |



### for-direction

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



### getter-return

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



### no-async-promise-executor

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



### no-await-in-loop

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



### no-compare-neg-zero

如果和`-0`机械能比较的时候，会展示警告信息，因为这种比较不会得到预期的效果。`x === -0` 的比较，在x于`+0`, `-0`的时候都会通过，所以最好的方式是使用`Object.is(x, -0)`.

##### 错误demo

```
if (x === -0) {
    // doSomething()...
}
```

##### 正确demo

```
if (x === 0) {
    // doSomething()...
}
或者
if (Object.is(x, -0)) {
    // doSomething()...
}
```



---



### no-cond-assign

在条件判断语句中，非常容易的把比较的操作符`==` 误写为一个赋值操作符(`=`)。

```
// Check the user's job title
if (user.jobTitle = "manager") {
    // user.jobTitle is now incorrect
}
```

这种语句是没有逻辑错误的，但是这样写可能会导致意图的混淆，可能原来的想法是等号操作符而不是赋值。

##### options

* except-parens: 需要包在括号里面，且仅在while, do...while等循环中允许这种赋值
* always: disallow all assignment in test conditions.

##### 错误demo: except-parens

```
// Unintentional assignment
var x;
if (x = 0) {
    var b = 1;
}

// Practical example that is similar to an error
function setHeight(someNode) {
    "use strict";
    do {
        someNode.height = "100px";
    } while (someNode = someNode.parentNode);
}
```

##### 正确demo: except-parens

```
// Assignment replaced by comparison
var x;
if (x === 0) {
    var b = 1;
}

// Practical example that wraps the assignment in parentheses
function setHeight(someNode) {
    "use strict";
    do {
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode));
}

// Practical example that wraps the assignment and tests for 'null'
function setHeight(someNode) {
    "use strict";
    do {
        someNode.height = "100px";
    } while ((someNode = someNode.parentNode) !== null);
}

```

always就是所有情况下都不允许使用赋值进行条件判断。



---



### no-console

禁止使用console

javascript语言是在浏览器上运行的，然后作为一个最佳的实践，就是不要带上console方法。因为这个方法是以调试为目的，因此不适合直接输出到客户端。一般来说，console方法都需要在production包中过滤掉。



##### options

* allow : 用一个字符串数组来定义哪些console方法是允许的。



##### When Not To Use It

在nodejs和某些情况下，如果必须要使用console方法，可以把这个规则校验关掉。



---



### no-constant-condition

禁止在条件中使用常量表达式：if(true) if(false)

##### 错误demo

```
if (false) {
    doSomethingUnfinished();
}
```

##### 正确demo

```
if (x === 0) {
    doSomething();
}
```

##### options

* checkLoops, 默认是true的，设置为false的话，就可以在loop循环中使用常量

```
while (true) {
    doSomething();
    if (condition()) {
        break;
    }
};
```



---



### no-control-regex

不允许在正则表达式中使用控制字符

在ASCII码表中，十进制的0-31是特殊的控制字符。这些字符在javascript中很少用到，如果在正则表达式中用到，几乎可以判断是出错了。

ASCII码表

| 二进制   | 十进制 | 十六进制 | 字符/缩写          | 解释       |
| -------- | ------ | -------- | ------------------ | ---------- |
| 00011111 | 31     | 1F       | US(Unit Separator) | 单元分隔符 |
| 00100000 | 32     | 20       | (Space)            | 空格       |

##### 错误demo

```
var pattern1 = /\x1f/;
var pattern2 = new RegExp("\x1f");

// 这边的\x1f是单元分隔符，所以就判断是错误
```

##### 正确demo

```
var pattern1 = /\x20/;
var pattern2 = new RegExp("\x20");
```

##### When Not To Use It

如果确实需要使用控制字符来匹配，可以关掉该规则。





