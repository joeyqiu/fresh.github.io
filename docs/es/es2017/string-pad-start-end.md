#### String.prototype.padStart/String.prototype.padEnd

### 起因

因为没有一个合适的原生方法来给字符串进行填充，所以string的操作越来越难，相比别的语言就会觉得并不完整。

也是因为在通常情况下，在大部分的浏览器和框架中，都有字符串填充的函数存在。比如FirefoxOS中的每个app都实现了一个在左边填充字符串的函数（也是因为需要用到）。



#### String.prototype.padStart()

**`padStart()`** 方法用另一个字符串填充当前字符串(如果需要的话，会重复多次)，以便产生的字符串达到给定的长度。从当前字符串的左侧开始填充。

用法：`str.padStart(targetLength [, padString])`

参数：

* targetLength: 当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
* padString[可选]：填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断。此参数的默认值为 " "（U+0020）。

返回值：在原字符串开头填充指定的填充字符串直到目标长度所形成的新字符串。

```javascript
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"
```

##### Polyfill

```javascript
if (!String.prototype.padStart) {
    String.prototype.padStart = function padStart(targetLength,padString) {
        targetLength = targetLength>>0; //floor if number or convert non-number to 0;
        padString = String((typeof padString !== 'undefined' ? padString : ' '));
        if (this.length > targetLength) {
            return String(this);
        }
        else {
            targetLength = targetLength-this.length;
            if (targetLength > padString.length) {
                padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
            }
            return padString.slice(0,targetLength) + String(this);
        }
    };
}
```



#### String.prototype.padEnd()

**`padEnd()`** 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。

用法：str.padEnd(targetLength [, padString])

参数和返回值同上

```javascript
'abc'.padEnd(10);          // "abc       "
'abc'.padEnd(10, "foo");   // "abcfoofoof"
'abc'.padEnd(6, "123456"); // "abc123"
'abc'.padEnd(1);           // "abc"
```

##### Polyfill

```javascript
if (!String.prototype.padEnd) {
    String.prototype.padEnd = function padEnd(targetLength,padString) {
        targetLength = targetLength>>0; //floor if number or convert non-number to 0;
        padString = String((typeof padString !== 'undefined' ? padString: ''));
        if (this.length > targetLength) {
            return String(this);
        }
        else {
            targetLength = targetLength-this.length;
            if (targetLength > padString.length) {
                padString += padString.repeat(targetLength/padString.length); //append to original to ensure we are longer than needed
            }
            return String(this) + padString.slice(0,targetLength);
        }
    };
}
```



##### 命名

在2015年12月份的TC39会议上，决定了填充方法的名字为：`padStart/padEnd`， 为了统一性，甚至把`trimLeft/trimRight`的名字都改成了`trimStart/trimEnd` ，当然为了兼容，就的方法名还会存在。



##### ‘min length’ VS ‘max length’

关于第一个参数，有讨论过决定是填充字符串的最小长度(minimum length) 或者最大长度(maximum length)。当选用最小长度时：`'foo'.padEnd(4, '12')`的结果是`foo12`，当选用最大长度时，该方法的最终结果是`foo1`。考虑到方法主要是为了格式化列中的等宽文本，所以才用了最大长度。而且如果要实现最小长度的功能，可以使用`String.prototype.repeat`方法。



#### Left padding, with respect to the fill string: consistency with other languages

发现的支持给字符串在左右进行填充的语言只有Ruby和PHP。他们进行填充都是从字符串的开始进行填充。

```javascript
"abc".padStart(10, "0123456789");  // 0123456abc

// 如果用字符串的末尾进行填充，结果会是：3456789abc
```

为了和别的语言保持一致，所以这边也采用开始部分进行填充。
