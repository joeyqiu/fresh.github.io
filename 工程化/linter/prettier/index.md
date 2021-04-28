# Prettier

https://prettier.io/docs/en/index.html



## Prettier vs linters

Linters有两种类型的规则:

**Formatting rules**: eg: [max-len](https://eslint.org/docs/rules/max-len), [no-mixed-spaces-and-tabs](https://eslint.org/docs/rules/no-mixed-spaces-and-tabs), [keyword-spacing](https://eslint.org/docs/rules/keyword-spacing), [comma-style](https://eslint.org/docs/rules/comma-style)…

Prettier alleviates the need for this whole category of rules! Prettier is going to reprint the entire program from scratch in a consistent way, so it’s not possible for the programmer to make a mistake there anymore :)

Prettier在格式化规则方面提供帮助。

**Code-quality rules**: eg [no-unused-vars](https://eslint.org/docs/rules/no-unused-vars), [no-extra-bind](https://eslint.org/docs/rules/no-extra-bind), [no-implicit-globals](https://eslint.org/docs/rules/no-implicit-globals), [prefer-promise-reject-errors](https://eslint.org/docs/rules/prefer-promise-reject-errors)…

Prettier does nothing to help with those kind of rules. They are also the most important ones provided by linters as they are likely to catch real bugs with your code!

Prettier在代码质量检测方面不做任何事情。

In other words, use **Prettier for formatting** and **linters for catching bugs!**

所以Prettier只是格式化的工具，而linter则是用来捕获bug的。



## 安装

```
npm install --save-dev --save-exact prettier

or

yarn add --dev --exact prettier
```

然后再项目的根目录下创建一个空的配置文件，让别的工具知道在使用Prettier

```
echo {}> .prettierrc.json
```

然后在创建一个`.prettierignore`文件，来声明哪些文件是不需要进行格式化的。如

```
# Ignore artifacts
build
coverage
```

再进行格式化的命令

```
npx prettier --write .

or 

yarn prettier --write .
```



### eslint

如果项目中也使用了eslint，需要安装个`eslint-config-prettier`插件来避免eslint和prettier之间可能发生的冲突。这个插件会关闭所有eslint中会和prettier可能冲突的配置。

如果使用了styleint，也有个类似的插件：`stylelint-config-prettier`

#### eslint-config-prettier

https://github.com/prettier/eslint-config-prettier#installation

这边的配置文件可以看下，还有不少配置介绍到： `prettier/react`等。。。

```
npm install --save-dev eslint-config-prettier
```

安装好之后，写在eslint的配置文件`.eslintrc.*`的`extends` 数组中。并且确保是放在__最后面__的，因此可以覆盖掉别的配置。

```
{
	"extends": [
		"some-other-config-you-use",
		"prettier"
	]
}
```



### Git 钩子

一般项目中都是在钩子中使用prettier，在commit之前， husky的使用比较麻烦，到时候需要看下官方文档，要注意不同版本的差别。

```
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "**/*": "prettier --write --ignore-unknown"
  }
}
```

> 如果使用了Eslint，必须确保lint-staged在prettier之前运行，而不是之后。prettier需要是最后进行格式化的。



## 配置文件

Prettier使用[cosmiconfig](https://github.com/davidtheclark/cosmiconfig){:target="_blank"}来支持配置文件，这就意味着你可以按照如下先后顺利来配置:

* 在`package.json 文件中配置`prettier` 属性
* 使用JSON或者YAML写一个`.prettierrc`文件
* 一个`.prettierrc.json`,  `.prettierrc.yml`, `.prettierrc.yaml`, `.prettierrc.json5` 文件
* 一个`.prettierrc.js`, `.prettierrc.cjs`, `.prettier.config.js`, `.prettier.config.cjs`文件，使用`module.exports`导出一个对象
* 一个`.prettierrc.toml` 文件。



### .prettierignore

推荐在项目中放置一个`.prettierignore` 文件。确保不会改动到那些你不想要修改的文件。