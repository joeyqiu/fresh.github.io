## react-scripts

在create-react-app的脚本中，通过如下命令开始react-scripts脚本的执行

```
var init = require('${packageName}/scripts/init.js');
init.apply(null, JSON.parse(process.argv[1]));
```

所以调用的是在scripts/init.js中通过module.exports命令导出的函数。



init.js中的执行

- 给package.json中添加更多的配置数据(dependencies, scripts, eslintConfig,browserslist...)
- 如果目录下有旧的README.md文件，把README.md => README.old.md
- 最重要的一步，把`template`目录下的模版文件都拷贝到目录下去(两种模版：一种是使用typescript的，一种是普通的)
- 把目录中的gitignore改为.gitignore
- 安装依赖包，如react, react-dom之类
- 初始化git，并添加第一条commit注释
- 输出打印提示语句。





其实最终总结下来，init函数的作用，就是把template中的文件拷贝到创建的项目下面，然后添加一个package.json文件。项目中的node_modules目录，也就是package.json中的依赖，是在`create-react-app`中install函数中做的执行。



剩余的一些脚本执行，都是在react-script中的， 可以在react-script目录中去记录。







###  一些函数

tryGitInit

只是做了git的初始化工作和初始的commit信息，，远程仓库之类的需要自己设置，创建了远程的仓库才能够push

```
function tryGitInit(appPath) {
  let didInit = false;
  try {
    execSync('git --version', { stdio: 'ignore' });
    if (isInGitRepository() || isInMercurialRepository()) {
      return false;
    }

    execSync('git init', { stdio: 'ignore' });
    didInit = true;

    execSync('git add -A', { stdio: 'ignore' });
    execSync('git commit -m "Initial commit from Create React App"', {
      stdio: 'ignore',
    });
    return true;
  } catch (e) {
    if (didInit) {
      // If we successfully initialized but couldn't commit,
      // maybe the commit author config is not set.
      // In the future, we might supply our own committer
      // like Ember CLI does, but for now, let's just
      // remove the Git files to avoid a half-done state.
      try {
        // unlinkSync() doesn't work on directories.
        fs.removeSync(path.join(appPath, '.git'));
      } catch (removeErr) {
        // Ignore.
      }
    }
    return false;
  }
}
```

