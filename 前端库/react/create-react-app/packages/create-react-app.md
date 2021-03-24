# Create-react-app

```
var currentNodeVersion = process.versions.node;
var semver = currentNodeVersion.split('.');
var major = semver[0];

if (major < 8) {
  console.error(
    'You are running Node ' +
      currentNodeVersion +
      '.\n' +
      'Create React App requires Node 8 or higher. \n' +
      'Please update your version of Node.'
  );
  process.exit(1);
}
```

检查node的版本先，如果版本低于8，则退出。

总的来说，主要是做一些各种版本的判断，npm/yarn等使用哪个，那些版本，，需要安装哪些包啊，默认的话就是三个：`react, react-dom, react-script`，如下所示：

```
"dependencies": {
    "react": "^16.9.0",
    "react-dom": "^16.9.0",
    "react-scripts": "3.1.1"
  }
```



并最终通过executeNodeScript来执行`react-script`包中的`init.js`脚本





### 命令流程

- 通过commander来给命令做配置
- projectName是必须的判断
- 调用createApp函数
  - 检查包名是否合适(是否是一个正确的npm包名)
  - 确保包名目录的存在
  - 检查当前目录环境是否安全(isSafeToCreateProjectIn)
  - 通过fs.writeFileSync函数写入package.json文件
  - 判断npm是否可以使用（checkThatNpmCanReadCwd）
  - 检查Node版本，需要>=8.10.0，如果node 版本过低，把react-script改成低版本的
  - 判断并检查npm, yarn的版本问题
  - 把yarn.lock.cached文件拷贝到包目录下当作yarn.lock
  - 调用run函数
- 调用run函数
  - 得到安装的react-script包(通过getInstallPackage函数来确认具体版本)
  - 得到安装的依赖包名
  - 得到包名
  - checkIfOnline，检查状态
  - 调用install函数来安装依赖的包
- 调用install 函数来正式安装
  - Yarn/npm的安装命令执行。spawn
- 一些数据检查
- 执行node脚本，并执行`react-script/script/init.js`脚本
- 如果遇到冲突===》废弃安装，并删除一些不需要的文件夹。



```
await executeNodeScript(
          {
            cwd: process.cwd(),
            args: nodeArgs,
          },
          [root, appName, verbose, originalDirectory, template],
          `
        var init = require('${packageName}/scripts/init.js');
        init.apply(null, JSON.parse(process.argv[1]));
      `
        );
```





## create-react-app中的一些函数解析



### isSafeToCreateProjectIn(root, name)

```
如果是错误的安装，会创建下述文件，并在下次安装过程中删除。
const errorLogFilePatterns = [
  'npm-debug.log',
  'yarn-error.log',
  'yarn-debug.log',
];

onst validFiles = [
  '.DS_Store',
  'Thumbs.db',
  '.git',
  '.gitignore',
  '.idea',
  'README.md',
  'LICENSE',
  '.hg',
  '.hgignore',
  '.hgcheck',
  '.npmignore',
  'mkdocs.yml',
  'docs',
  '.travis.yml',
  '.gitlab-ci.yml',
  '.gitattributes',
];

const conflicts = fs
    .readdirSync(root)
    // 读取root目录下的所有文件,并过滤掉上述正当的文件
    .filter(file => !validFiles.includes(file))
    // IntelliJ IDEA creates module files before CRA is launched
    .filter(file => !/\.iml$/.test(file))
    // Don't treat log files from previous installation as conflicts
    .filter(
      file => !errorLogFilePatterns.some(pattern => file.indexOf(pattern) === 0)
    );
    

```



### shouldUseYarn

```
const useYarn = useNpm ? false : shouldUseYarn();

如果命令中指定了npm，那就用npm，否则再判断是否使用yarn

function shouldUseYarn() {
  try {
  判断是否要使用yarn
    execSync('yarnpkg --version', { stdio: 'ignore' });
    return true;
  } catch (e) {
    return false;
  }
}
```




