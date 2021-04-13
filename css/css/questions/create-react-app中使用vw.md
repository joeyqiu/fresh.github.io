# 在create-react-app中使用vw

这边使用vw作为手机端适配的解决方案，然后在create-react-app的脚手架中配置使用



#### create-react-app创建项目

官网地址：https://github.com/facebook/create-react-app

```shell
npx create-react-app my-app
```

上述命令创建一个react 项目



在项目的node_modules中有一个`react-script`的项目，这边的脚本命令主要就是其中添加。

其实可以把该项目移出来，放在根目录下，这边就还是放在原来的位子吧



#### 在react-script中配置



在该目录下，依次按照如下插件

````json
"postcss-aspect-ratio-mini": "0.0.2",
"postcss-cssnext": "^3.1.0",
"postcss-px-to-viewport": "0.0.3",
"postcss-viewport-units": "^0.1.4",
"postcss-write-svg": "^3.0.1",
"cssnano": "^4.0.5",
"cssnano-preset-advanced": "^4.0.0",

这边两个是之前已经安装的
"postcss-flexbugs-fixes": "3.2.0",
"postcss-loader": "2.0.8",
````

然后在`config/webpack.config.dev.js`中做下修改配置

```javascript
{
                loader: require.resolve('postcss-loader'),
                options: {
                  // Necessary for external CSS imports to work
                  // https://github.com/facebookincubator/create-react-app/issues/2677
                  ident: 'postcss',
                  plugins: () => [
                    require('postcss-flexbugs-fixes'),
                    autoprefixer({
                      browsers: [
                        '>1%',
                        'last 4 versions',
                        'Firefox ESR',
                        'not ie < 9', // React doesn't support IE8 anyway
                      ],
                      flexbox: 'no-2009',
                    }),
                    require("postcss-aspect-ratio-mini"),
                    require("postcss-write-svg")({utf8: false}),
                    require("postcss-px-to-viewport")({
                        viewportWidth: 750,
                        viewportHeight: 1334,
                        unitPrecision: 5,
                        viewportUnit: 'vw',
                        selectorBlackList: ['.ignore', '.hairlines'],
                        minPixelValue: 1,
                        mediaQuery: false
                    }),
                    require("postcss-viewport-units")({
                        /* 过滤原本带有content属性的元素 */
                        filterRule: (rule) => {          
                            return rule.selector.indexOf('::after') < 0 && rule.selector.indexOf(':after') < 0 && rule.selector.indexOf('::before') < 0 && rule.selector.indexOf(':before') < 0;
                        }
                    }),
                    require("cssnano")({
                        preset: "advanced",
                        autoprefixer: false,
                        "postcss-zindex": false,
                        reduceIdents: false
                    })
                  ],
                },
              }
```

最终代码如上，，哦，还需要在旁边的`webpack.config.prod.js`中拷贝一份



#### vw的兼容

在caniuse的网站上可以看到，移动大部分都已经兼容viewport单位了，为了兼容少部分手机，可以通过`viewport-units-buggyfill`来兼容。上面的`postcss-viewport-units`插件就是为了兼容做的准备。



`postcss-viewport-units`给所有vw的元素中主动添加content变量，为buggyfill提供便利。



在页面中引入buggyfill.hacks.js文件，然后在body底部添加下面的代码

```html
<script> 
	window.onload = function () {
		window.viewportUnitsBuggyfill.init({
			hacks: window.viewportUnitsBuggyfillHacks });
	} 
</script>

```





## less配置

还可以按照上述方法，配置下less的处理，依次安装

```json
"less": "^3.8.0",
"less-loader": "^4.1.0",
```

然后在配置文件中添加less的匹配`test: /\.css|less$/,`，在添加less-loader

```javascript
{
	loader: require.resolve('less-loader')
}
```

放在`css|less`loader的最后就行

