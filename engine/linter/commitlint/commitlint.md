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

这边用到了husky。

需要注意是的提交的顺序，需要配置在`commitmsg` 中，如果在`precommit` 中进行commitlint的处理，会一直接受到之前的旧commit消息，导致失败。



git会自带多个钩子，但是具体钩子的实现，需要自己来写，所以可以使用大佬们编写好的钩子方法，来做校验。

自带钩子在`.git/hooks`  下面，以`.sample` 结尾，如果需要生效，把`.sample` 去掉就行。但是具体里面执行了么也不是很懂，就用别人提供的工具吧。



### 参考资料

https://github.com/conventional-changelog/commitlint/#what-is-commitlint

https://www.conventionalcommits.org/en/v1.0.0/

https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines