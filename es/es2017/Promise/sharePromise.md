Promise介绍：https://es6.ruanyifeng.com/#docs/promise
Promise规范：https://www.ituring.com.cn/article/66566

# Promise 基本用法

```javascript

const ps = new Promise((res, rej) => {
  res('res')
})

ps
  .then(res => {
  console.log(res)
})
  .then(res => {
  console.log(res)
})
```

```javascript

const ps = new Promise((res, rej) => {
  res('res')
})

ps.then(res => {
  console.log(res)
})

ps.then(res => {
  console.log(res)
})
```

```javascript
const ps = new Promise((res, rej) => {
  res('res')This 
})

ps.then(res => {
  return new Promise((resolve, reject) => {
    resolve(res)
  })
}).then(res => {
  console.log(res)
})
```

# Promise 基本特性

1. Promise和事件不一样，状态随用随取
2. Promise从pending -> resolved / rejected之后不能再变
3. 并不能获取Promise的进度
4. Promise不能取消

