## git commit信息的规范化

安装

```
npm install -g @commitlint/cli @commitlint/config-conventional
```

配置

```
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

就用这个命令在项目中生成一个配置文件。

在package.json中配置

```
"precommit": "lint-staged && pretty-quick --staged",
"commitmsg": "commitlint --edit",
```

这边没用到husky，而是用的自带的钩子。

需要注意是的提交的顺序，需要配置在`commitmsg` 中，如果在`precommit` 中进行commitlint的处理，会一直接受到之前的旧commit消息，导致失败。



### 参考资料

https://github.com/conventional-changelog/commitlint/#what-is-commitlint

https://www.conventionalcommits.org/en/v1.0.0/

https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines