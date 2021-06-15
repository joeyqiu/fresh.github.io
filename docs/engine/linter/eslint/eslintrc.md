## eslintrc配置

可以选用js/json等各种格式的配置文件，这边采用的是js格式



```
module.exports = {
		"parser": "babel-eslint",
		"plugins": ["react"],
		"settings": {
	    "react": {
	      "version": "16.9.0"
	    }
	  },
    "extends": ["eslint:recommended", "plugin:react/recommended"],
    "root": true,
    "env": {
	    "es6": true,
	    "browser": true,
	    "node": true,
	    "jest": true
	  },
	  
	 /** 这部分可以用env中的jest取代  "overrides": [{
      "files": [
        "**/*.test.js",
        "**/*.test.jsx"
      ],
      "env": {
        "jest": true
      }
    }],*/
    "rules": {
    }
};
```



### babel-eslint

eslint允许自定义的解析，因为有时候使用了比较新的语法，然后babel支持了，但是eslint并不支持，所以需要使用这个插件。



### standard

这边继承自最基本的规则集



### plugins

`plugins: react` ， React指定的一些Eslint规则，可以避免React代码的一些无法通过。

声明了plugins之后，还需要在extends中继承一下。



### settings

支持在配置文件中添加共享设置。可以添加`settings`对象到配置文件，它将提供给每一个将被执行的规则。

```
"react": {
	      "version": "16.9.0"
	    }
```

这边用来指定react的版本，提供给`eslint-plugin-react` 插件使用



### env

一个环境定义了一组预定义的全局变量。可用环境很多，这边列下上面用到的：

* browser: 浏览器环境中的全局变量
* node: Nodejs全局变量和Nodejs作用域
* es6: 启动除了modules以外的所有ECMAScript6特性（该选项会自动设置ecmaVersion解析器选项为6）
* jest: Jest全局变量



### overrides

更精细的配置，可以应用于指定的测试代码文件



### rules

可以在配置文件中，通过直接设置off，来关闭某些配置

```
"no-constant-condition": "off"
```

* "off"或"0" - 关闭规则
* "warn"或"1" - 开启规则，使用警告级别的错误： warn（不会导致程序退出）
* "error" 或 "2" - 开启规则，使用错误级别的错误：error（当被触发的时候，程序会退出）