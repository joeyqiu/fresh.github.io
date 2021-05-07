## eslint recommended rules

https://eslint.org/docs/rules/

这边是eslint官方的一些规则。



### Possible Errors

可能的语法和逻辑错误

| 规则名称                        | 规则说明                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| [for-direction](#for-direction) | enforce "for" loop update clause moving the counter in the right direction. |



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

