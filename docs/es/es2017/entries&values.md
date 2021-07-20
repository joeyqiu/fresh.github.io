#### Object.entries()

此方法签名如下：

```typescript
Object.entries(value : any) : Array<[string,any]>
```

如果 JavaScript 数据结构具有键和值，条目是一个键值对，被编码为2元数组。`Object.entries(x)` 强制 `x` 为一个对象，并返回一个给定对象自己的可枚举属性[key，value]对的数组：

```javascript
let obj = { one: 1, two: 2 };
for (let [k,v] of Object.entries(obj)) {
    console.log(`${JSON.stringify(k)}: ${JSON.stringify(v)}`);
}
// Output:
// "one": 1
// "two": 2
```

其键为 symbols 的属性被忽略：

```javascript
Object.entries({ [Symbol()]: 123, foo: 'abc' });
// [ [ 'foo', 'abc' ] ]
```



#### 通过 Object.entries() 设置为 Map 对象

Object.entries() 还允许你将一个对象设置为 Map 对象。这比使用2元数组的数组更简洁，但是键只能是字符串。

```javascript
let map = new Map(Object.entries({
    one: 1,
    two: 2,
}));
console.log(JSON.stringify([...map]));
// [["one",1],["two",2]]
```



---



#### Object.values()

此方法签名如下：

```typescript
Object.values(value : any) : Array<any>
```

它的工作原理很像 Object.entries() ，但是，正如其名称所示，它只返回自己可枚举的字符串键属性的值：

```javascript
Object.values({ one: 1, two: 2 }); // [ 1, 2 ]
```





#### 常见问题

- 为什么 Object.entries() 的返回值是一个数组而不是一个迭代器？

在这种情况下的相关先例是 `Object.keys()` ，而不是，例如 `Map.prototype.entries()`。

- 为什么 Object.entries() 只返回自己的可枚举的字符串键属性？

再次说明，这样做与 `Object.keys()` 一致。该方法也忽略其 symbols 键的属性。最终，可能会有一个返回所有属性的 Reflect.ownEntries() 方法。
