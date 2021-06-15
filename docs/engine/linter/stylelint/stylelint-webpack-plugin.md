## stylelint-webpack-plugin

在webpack中使用stylelint



需要先安装如下三个包：

```
stylelint, stylelint-webpack-plugin, stylelint-config-recommended
```

这边config-x可以是`recommended` 或者是 `standard`，依据具体情况使用



目前是用的react脚手架，直接在webpack.config.js中添加配置

```
const StylelintPlugin = require('stylelint-webpack-plugin');

plugins: [
	new StylelintPlugin({
        context: 'src',
        files: '**/*.less',
        emitWarning: true,
        configFile: path.resolve(__dirname,'../stylelint.config.js')
      })
]
```



然后再目录下添加`stylelint.config.js` 文件，或者根据文档，创建别的配置文件



### stylelint.config.js

```
module.exports = {
	"extends": "stylelint-config-recommended",
	"rules": {
		xxx
	}
}
```

