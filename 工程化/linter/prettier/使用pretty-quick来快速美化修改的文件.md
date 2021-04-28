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

如果项目中还安装了eslint，与eslint的配合安装`eslint-config-prettier`插件，请看介绍页面。



### 参数介绍

#### `--staged` 

Pre-commit mode. Under this flag only staged files will be formatted, and they will be re-staged after formatting.

Partially staged files will not be re-staged after formatting and pretty-quick will exit with a non-zero exit code. The intent is to abort the git commit and allow the user to amend their selective staging to include formatting fixes.