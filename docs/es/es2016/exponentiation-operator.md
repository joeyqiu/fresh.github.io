求幂运算符，一种将指数应用于基数的数学计算。已有的`Math.pow()`方法可以执行求幂计算，但它也是为数不多的需要通过方法而不是正式的运算符来进行求幂运算的语言之一。

求幂运算符是两个星号(**)：左操作数是基数，右操作数是指数。

```javascript
// x ** y

let squared = 2 ** 2;
// same as: 2 * 2
// also same as, Math.pow(2,2);

let cubed = 2 ** 3;
// same as: 2 * 2 * 2
// also same as : Math.pow(2,3)
```

```javascript
// x **= y

let a = 2;
a **= 2;
// same as: a = a * a;

let b = 3;
b **= 3;
// same as: b = b * b * b;
```



#### 运算顺序

求幂运算符在Javascript所有二进制运算符中具有最高的优先级（一元运算符的优先级高于**），这意味着它首先应用于所有复合操作。

```javascript
let result = 2 * 5 ** 2;
console.log(result); // 50
```

先计算Math.pow(5, 2)，然后乘以2，得出50。



#### 运算限制

左侧的一元表达式只能使用++或--。

```javascript
// 语法错误
let result = -5 ** 2;
```

-是只适用于5呢，还是表达式5**2的结果，这边的语法有问题，含义不清，所以会报错。

如果需要明确意图，需要用括号包裹-5，或5**2。

```javascript
let result = -(5 ** 2);

let result1 = (-5) ** 2;
```

上述两个意图明确的表达式就是正确的。
