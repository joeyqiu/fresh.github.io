## 使用pretty-quick来快速美化修改的文件

pretty-quick: https://www.npmjs.com/package/pretty-quick

pretty-quick可以用来美化修改过的文件，如果你本地并不想时刻跑着prettier，然后我们可以加个钩子，在`git commit` 的时候，进行pretty的美化。

### 安装

```
yarn add --dev prettier pretty-quick
```

### 使用

一般在commit的钩子上使用上，所以还要安装个husky，推荐使用 husky-hg，比较的方便，husky不知配置，还要命令行处理，就麻烦了

```
yarn add husky-hg --dev
```

 然后再`package.json ` 中加入如下内容：

```
{
  "scripts": {
    "precommit": "pretty-quick --staged",
    "...": "..."
  }
}
```

如果项目中还安装了eslint，与eslint的配合安装`eslint-config-prettier`插件(Turns off all rules that are unnecessary or might conflict with [Prettier])，具体请看插件介绍页面。



### eslint-config-prettier

https://github.com/prettier/eslint-config-prettier

Turns off all rules that are unnecessary or might conflict with [Prettier](https://github.com/prettier/prettier). 关闭eslint中，所有可能与prettier冲突的语法。一般都是设为false，空，0等值。

This lets you use your favorite shareable config without letting its stylistic choices get in the way when using Prettier.

Note that this config *only* turns rules *off,* so it only makes sense using it together with some other config.

具体说明可以看文档，这边主要是有个注意点列出来，因为是版本的问题

> Note: You might find guides on the Internet saying you should also extend stuff like `"prettier/react"`. Since version 8.0.0 of eslint-config-prettier, all you need to extend is `"prettier"`! That includes all plugins.

就是说，旧版本8.0之前，会需要`prettier/react` 等语法，8.0之后，直接extends`prettier`就好了。



### 参数介绍

#### `--staged` 

Pre-commit mode. Under this flag only staged files will be formatted, and they will be re-staged after formatting.

Partially staged files will not be re-staged after formatting and pretty-quick will exit with a non-zero exit code. The intent is to abort the git commit and allow the user to amend their selective staging to include formatting fixes.