## code_part1

通过`webpack(config)`的方式来调用webpack打包编译。

在webpack的库文件中，通过如下方式导出：

```
exports = module.exports = webpack;
```



接下来看下webpack这个函数中的代码

```
const webpack = (options, callback)=>{}
```

webpack接受一个配置对象，和一个callback函数【这个函数是可选的】



把传入的配置对象进行前置的验证下，通过`ajv`库，来要求传入的对象是一个符合规范的对象，，，否则抛出错误信息

```
const webpackOptionsValidationErrors = validateSchema(
		webpackOptionsSchema,
		options
);
if (webpackOptionsValidationErrors.length) {
		throw new WebpackOptionsValidationError(webpackOptionsValidationErrors);
}
```



如果传入的options对象其实是一个数组，通过`Array.from`方法来遍历这个数组，然后把数组中的对象进行webpack处理，，，然后返回给MultiCompiler对象

```
if (Array.isArray(options)) {
		compiler = new MultiCompiler(
			Array.from(options).map(options => webpack(options))
		);
	}
```



如果传入的options就是一个对象，进行options的处理



### 传入的options处理

```
options = new WebpackOptionsDefaulter().process(options);
```

WebpackOptionsDefaulter中通过this.set方法设置了一些默认的配置项

```
this.set("entry", "./src");
...

// 如果没有默认值，都赋值到defaults对象上, 如果有默认值，把默认值赋值到defaults对象上，然后config对象传入设置的值。。。就是保证defaults对象包含所有参数
set(name, config, def) {
		if (def !== undefined) {
			this.defaults[name] = def;
			this.config[name] = config;
		} else {
			this.defaults[name] = config;
			delete this.config[name];
		}
	}
```

在`process`方法中遍历defaults对象，有`call`, `make`, `append` 和值为`undefined` 四种处理

这么看来，WebpackOptionsDefaulter中是设置了兜底必须要有的options值，然后如果传入的options对象缺少值，这边就添加上去。





创建compiler对象, options.context指的是解析的入口起点，是一个基础目录的绝对路径

```
compiler = new Compiler(options.context);
```

