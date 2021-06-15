## 在ios8下的兼容性问题

下面这段代码， 在ios8下会报错，因为`window.performance`需要从ios9才开始兼容

```
var 
c=window,
d=c.performance.now||c.performance.mozNow||c.performance.msNow||c.performance.oNow||c.performance.webkitNow;
```



修改为：

```
var 
c=window,
d=c.performance&&(c.performance.now||c.performance.mozNow||c.performance.msNow||c.performance.oNow||c.performance.webkitNow);
```



