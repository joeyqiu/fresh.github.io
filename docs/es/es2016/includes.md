#### 动机

当使用数组的时候，经常会需要判断数组中是否包含某个元素，然后经常使用如下用法：

```javascript
if (arr.indexOf(el) !== -1) {}

arr.indexOf(el) >= 0;

~arr.indexOf(el)
```

上述用法会存在两个问题：

* 用法含义不够清晰（fail to "say what you mean"）：上述方法是判断数组中第一个匹配上该元素的位置，用这种方式来决定你想问的问题：是否包含该元素
* `[NaN].indexOf(NaN) === -1`，所以对NaN是无效的。



#### 如何使用Array.prototype.includes()方法

Array.prototype.includes()方法接受两个参数：要搜索的值及开始搜索的索引位置，第二个参数是可选的。提供第二个参数时，includes()将从索引开始匹配（默认的开始索引位置为0）。如果在数组中找到要搜索的值，返回true，否则返回false。

```javascript
let values = [1,2,3];

console.log(values.includes(1)); // true
console.log(values.includes(0)); // false
// 从3开始搜查
console.log(values.includes(1, 2)); // false
```



#### 值的比较

用includes()方法进行值比较的时候，===操作符的使用有一个例外：即使`NaN === NaN`的计算结果为false，NaN也被认为是等于NaN，这与indexOf()方法的行为不同，或者严格使用===进行比较。

```javascript
let values = [1, NaN, 2];

console.log(values.indexOf(NaN)); // -1
console.log(values.includes(NaN)); // true
```

还有个奇怪之处，+0与-0被认为是相等的。在这种情况下，indexOf()和includes()的表现行为一致：

```javascript
let values = [1, +0, 2];

console.log(values.indexOf(-0)); // 1
console.log(values.includes(-0)); // true
```

>Includes uses the SameValueZero comparision algorithm instead of Strict Equalify Comparison, thus make [NaN].includes(NaN) true.



常见null，undefined等值的比较

```javascript
NaN == NaN // false
NaN === NaN // false
NaN == undefined // false
NaN === undefined // false
null == undefined // true
null === undefined // false
undefined == undefined // true
undefined === undefined // true
null == null // true
null === null // true
```



#### FAQ

##### why `includes` instead of `has` ?

has方法和contains方法一样，[is not web-compatible](http://esdiscuss.org/topic/having-a-non-enumerable-array-prototype-contains-may-not-be-web-compatible)。has方法在概念上用于搜索keys，includes用于搜索值。

- Keys inside a key-value map: `Map.prototype.has(key)`, `WeakMap.prototype.has(key)`, `Reflect.has(target, propertyKey)`
- Sets, whose elements are conceptually both keys and values: `Set.prototype.has(value)`, `WeakSet.prototype.has(value)`, `Reflect.Loader.prototype.has(name)`
- Strings, which are conceptually maps from indices to code points: `String.prototype.includes(searchString, position)`

从统一性来说，最佳的就是和String保持一致，而不是Map或Set。
