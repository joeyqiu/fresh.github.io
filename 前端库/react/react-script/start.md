## yarn start

执行`yarn start`来启动通过`create-react-app`创建的项目。



指定开发环境

```
// Do this as the first thing so that any code reading it knows the right env.
process.env.BABEL_ENV = 'development';
process.env.NODE_ENV = 'development';
```



引入一些配置文件

```
// Ensure environment variables are read.
require('../config/env');

```



会先做一些Host，端口等校验，然后通过 `checkBrowsers`方法来校验浏览器是否已配置可用。

通过`choosePort`方法来判断端口是否可用，是否需要重新开启一个新的端口。



剩下的方法其实就是执行webpack，通过脚本的方式执行webpack, `webapck(config)`方法的方式。

```
const compiler = createCompiler({
      appName,
      config,
      devSocket,
      urls,
      useYarn,
      useTypeScript,
      webpack,
    });
```



再创建一个webpackDevServer对象来启动服务

```
const devServer = new WebpackDevServer(compiler, serverConfig);
```



用devServer来监听端口的执行，并且通过open库，使用指定浏览器来打开制定的url地址。

```
devServer.listen(port, HOST, err => {
      if (err) {
        return console.log(err);
      }
      if (isInteractive) {
        clearConsole();
      }
      xxxx

      console.log(chalk.cyan('Starting the development server...\n'));
      openBrowser(urls.localUrlForBrowser);
    });
```



监听命令行，如果需要终止，则停止进程。

```
['SIGINT', 'SIGTERM'].forEach(function(sig) {
      process.on(sig, function() {
        devServer.close();
        process.exit();
      });
    });
```





这边webpack的配置文件，来自config目录，并指定development环境

```
const configFactory = require('../config/webpack.config');
const config = configFactory('development');
```





### open

可以通过open来打开默认浏览器

### osascript

应该是mac专用的脚本命令



