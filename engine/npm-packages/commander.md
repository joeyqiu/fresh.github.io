## commander

the complete solution for `node.js` command-line interfaces。



文档链接：https://www.npmjs.com/package/commander

API Document: http://tj.github.io/commander.js/



### Declaring program variable(定义变量)

导出一个全局变量

```
const program = require('commander');
program.version('0.0.1')
```

对于大型的项目而言，可能需要在多个地方多种用途使用Commander，因此最好每次创建一个局部的command对象来使用

```
const commander = require('commander')
const program = new commander.Command();
program.version('0.0.1');
```



### Options

Options are defined with the `.option()` method, also serving as documentation for the options.

Options用来设置传递的参数



### Commands

you can specify (sub)commands for your top-level command using `.command`. There are two ways these can be implemented:

* using an action handler attached to the command
* as a separate executable file

```
// Command implemented using action handler (description is supplied separately to `.command`)
// Returns new command for configuring.
program
  .command('clone <source> [destination]')
  .description('clone a repository into a newly created directory')
  .action((source, destination) => {
    console.log('clone command called');
  });
```



### Specify the argument syntax

you use `.arguments` to specify the arguments for the top-level command, and for subcommands they are inlcuded in the `.command` call.

通过`arguments`方法来指定传递的参数

````
var program = require('commander');

尖括号表示必传参数，方括号表示可选的参数
program
  .version('0.1.0')
  .arguments('<cmd> [env]')
  .action(function (cmd, env) {
    cmdValue = cmd;
    envValue = env;
  });

program.parse(process.argv);

if (typeof cmdValue === 'undefined') {
  console.error('no command given!');
  process.exit(1);
}
console.log('command:', cmdValue);
console.log('environment:', envValue || "no environment given");
````



### Custom help

`--help`命令可以提供一些已经预置的帮助信息，或者你可以自己定义想要的帮助信息。

```
var program = require('commander');

program
  .version('0.1.0')
  .option('-f, --foo', 'enable some foo')
  .option('-b, --bar', 'enable some bar')
  .option('-B, --baz', 'enable some baz');

// must be before .parse() since
// node's emit() is immediate

program.on('--help', function(){
  console.log('')
  console.log('Examples:');
  console.log('  $ custom-help --help');
  console.log('  $ custom-help -h');
});

program.parse(process.argv);
```







Calling the `version` implicitly adds the `-V` and `--version` options to the command.





