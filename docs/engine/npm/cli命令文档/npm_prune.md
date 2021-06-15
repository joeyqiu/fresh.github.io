## npm prune

文档地址：https://docs.npmjs.com/cli-commands/prune.html

re move extraneous packages



此命令移除“无关”的包。如果提供了包名，那么只有名称匹配的那个包才会被移除。

无关的包指的是没有在父包的依赖关系列表中列出的包。



如果指定了 `--production` 参数，或者将 `NODE_ENV` 环境变量 设置为 `production`，这个命令将移除 `devDependencies` 配置信息中列出的包。设置 `--no-production` 将会取消 `NODE_ENV` 为 `production` 的设置。