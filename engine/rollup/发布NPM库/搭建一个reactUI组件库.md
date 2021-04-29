## 搭建一个reactUI组件库

一个UI组件库，和工具类库相比下，就有点区别， 需要考虑到样式的问题。



基本步骤还是`npm init` 然后添加各种依赖的库



### react语法处理

因为是react的组件库，所需要考虑到react的语法处理，因此babel的配置如下：

```
{
	"presets": [
    ["@babel/env", {"modules": false}],
    "@babel/preset-react"
  ],
  "plugins": [
    "@babel/plugin-proposal-class-properties",
    "@babel/plugin-syntax-object-rest-spread"
  ]
}
```

然后在rollup的配置中，如果配置的是iife或者umd的格式，还需要设置下全局变量。

```
When creating an iife or umd bundle, you will need to provide global variable names to replace your external imports via the output.globals option.
```

rollup中，babel的配置还是之前那样，只是需要忽略`node_modules` 中的部分

```
babel({
	exclude: 'node_modules/**' // only transpile our source code
}),
```



### css部分

考虑到样式部分，需要添加处理样式的依赖

```
import postcss from 'rollup-plugin-postcss'
import autoprefixer from 'autoprefixer'
import cssnano from 'cssnano'

plugins: [
	postcss({
			plugins: [autoprefixer, cssnano]
		}),
	...
]
```

postcss现在是通用的，一般都有，然后按照不同的less, scss语法，添加不同的处理，这边准备用less语法，所以需要在package.json中安装下less。

这边的autoprefixer可以进行进行语法的补充，自动给css添加兼容浏览器的各种前缀。但是需要和browserslist字段搭配使用，一般建议是放在package.json中，或者创建个`.browserlistrc`的配置文件，看自己的选择。

```
The best way to provide browsers is a .browserslistrc file in your project root, or by adding a browserslist key to your package.json.
```

这边直接放在了package.json里面，然后具体配置的参数，可以看官网

```
"browserslist": [
    "iOS >= 8",
    "Android >= 4.4"
  ]
```



cssnano则是用来压缩生成的css代码，生产环境都需要压缩。



### 配置文件

然后最终的配置文件：

```
import resolve from '@rollup/plugin-node-resolve'
import babel from 'rollup-plugin-babel'
import { terser } from "rollup-plugin-terser"
import postcss from 'rollup-plugin-postcss'
import autoprefixer from 'autoprefixer'
import cssnano from 'cssnano'

const env = process.env.NODE_ENV

const config = {
	input: 'src/LoadingToast/index.js',
	output: [
		{ name: 'LoadingToast', file: 'dist/LoadingToast.umd.js', format: 'umd', globals: { react: 'React', 'react-dom': 'ReactDOM' } },
		{ file: 'dist/LoadingToast.esm.js', format: 'es' }
	],
	plugins: [
		postcss({
			plugins: [autoprefixer, cssnano]
		}),
    resolve(),
    babel({
      exclude: 'node_modules/**' // only transpile our source code
    }),
    terser()
  ],
  external: ['react', 'react-dom']
};

export default config
```





## 遇到的问题

* css中的px单位？

因为是H5项目，使用的都是经过处理后的rem，所以库文件提供的是px的话，单位怎么处理？



解决方案目前列了几种：

1. 如果只是内部使用的话，然后确定只在手机端使用，直接在打包成插件的时候改成rem单位，不会有问题
2. 通用的话，可以考虑把js和css都单独打包，rollup配置的时候抽离样式代码，然后让使用方自己来按需引入，这样的话就要在readme文档中写好。