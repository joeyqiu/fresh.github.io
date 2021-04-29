## inquirer

文档地址：https://github.com/SBoudrias/Inquirer.js#readme



A collection of common interactive command line user interface.

在用户的命令行界面提供一些交互。

Version 4.x only supports Node 6 and over, otherwise please use version 3.x



### Usage

```
var inquirer = require('inquirer');
inquirer
  .prompt({
    /* Pass your questions in here */
  })
  .then(answers => {
    // Use user feedback for... whatever!!
  });
```



提供一个question对象，来与用户进行交互,





### Question

* type:  Type of the prompt, default: `input` - possible values: `input`, `confirm`, `list`, `rawlist`, `expand`, `checkbox`, `password`, `editor`



