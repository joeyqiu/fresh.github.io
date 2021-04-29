## Validate-npm-package-name

 文档地址：https://www.npmjs.com/package/validate-npm-package-name



Give me a string and I'll tell you if it's a valid nom package name.

This package exports a single synchronous function that takes a `string` as input and returns an object with two properties:

* `validForNewPackages`: Boolean
* `validForOldPackages`: Boolean



## Naming Rules

这边列出了`npm`包名应该遵守的规则：

* Package name length should be greater than zero. (包名的长度应该大于0)
* all the characters in the package name must be lowercase i.e. , no uppercase or mixed case names are allowed（包名的所有字符应该是小写的，大写或者混合大小写都是不允许的）
* package name can consist of hyphens （包名中可以包含连字符）
* package name must not contain any non-url-safe characters (since name ends up being part of a URL) （包名不能包含任何在url中不安全的字符，应该包名会带到url中去）
* Package name should not start with `.` or `_` （包名不能以`.`和`_`开头）
* Package name should not contain any leading or trailing spaces （包名前后都不能有空格）
* Package name should not contain any of the following characters: `~)('!*` （包名中不允许中如下字符： `~)('!*`）
* package name cannot be the same as a node.js/io.js core module nor a reserved/blacklisted name, for example, the following names are invalid:  （包名不能和node.js或io.js中的核心方法或者保留字和黑名单字相同，如下。。。）
  * http
  * stream
  * node_modules
  * favicon.ico
* Package name length connot exceed 214 （包名的长度不能超过214）