## eject

`yarn eject`

如果想自己控制配置文件，则可以通过该命令把配置相关的数据提取出来。



主要是就是把`react-script`中的`script`和`config`目录提取出来，放到项目文件夹下面。

并修改package.json，把`react-script`中的依赖都拷贝到项目的`package.json`中去。



### 解析

先判断该项目下的git仓库状态，如果有`untracked files or uncommitted changes`， 也就是有改动没有提交的话，会直接拒绝继续执行。



其实会移动如下目录中的代码

```
const folders = ['config', 'config/jest', 'scripts'];
```



在执行移动之前，先把目录下的所有文件获取一份

```
const files = folders.reduce((files, folder) => {
      return files.concat(
        fs
          .readdirSync(path.join(ownPath, folder))
          // set full path
          .map(file => path.join(ownPath, folder, file))
          // omit dirs from file list
          .filter(file => fs.lstatSync(file).isFile())
      );
    }, []);
```



通过`verifyAbsent`方法判断目录和目录下的文件是否存在，如果存在重复的，也是停止后续执行(怕覆盖用户的代码)，除非用户删除了。

```
function verifyAbsent(file) {
      if (fs.existsSync(path.join(appPath, file))) {
        console.error(
          `\`${file}\` already exists in your app folder. We cannot ` +
            'continue as you would lose all the changes in that file or directory. ' +
            'Please move or delete it (maybe make a copy for backup) and run this ' +
            'command again.'
        );
        process.exit(1);
      }
    }
```



创建对应的目录

```
folders.forEach(folder => {
      fs.mkdirSync(path.join(appPath, folder));
    });
```



拷贝文件内容，如果文件内容中有`@remove-file-on-eject`的部分，则忽略该文件。说明是eject中不需要拷贝到新项目下的文件。

`remove-on-eject-begin`和`remove-on-eject-end`，中间夹着的代码段，也是不需要拷贝过去的。

```
if (content.match(/\/\/ @remove-file-on-eject/)) {
        return;
      }
content =
        content
          // Remove dead code from .js files on eject
          .replace(
            /\/\/ @remove-on-eject-begin([\s\S]*?)\/\/ @remove-on-eject-end/gm,
            ''
          )
          // Remove dead code from .applescript files on eject
          .replace(
            /-- @remove-on-eject-begin([\s\S]*?)-- @remove-on-eject-end/gm,
            ''
          )
          .trim() + '\n';
```



最终命令行中会打印如下：

```
Copying files into /Users/xxx/react-demo
  Adding /config/env.js to the project
  Adding /config/modules.js to the project
  Adding /config/paths.js to the project
  Adding /config/pnpTs.js to the project
  Adding /config/webpack.config.js to the project
  Adding /config/webpackDevServer.config.js to the project
  Adding /config/jest/cssTransform.js to the project
  Adding /config/jest/fileTransform.js to the project
  Adding /scripts/build.js to the project
  Adding /scripts/start.js to the project
  Adding /scripts/test.js to the project
```



接下来做package.json文件夹的改动

```
const ownPackage = require(path.join(ownPath, 'package.json'));
// react-scripts这个库项目的package.json

// react-demo，也就是通过create-react-app创建的项目的package.json
const appPackage = require(path.join(appPath, 'package.json'));
```



如果appPackage中有`react-scripts`的依赖，需要先删除了。在devDependencies和dependencies中都判断下

```
const ownPackageName = ownPackage.name;
// 就是react-scripts
    if (appPackage.devDependencies) {
      // We used to put react-scripts in devDependencies
      if (appPackage.devDependencies[ownPackageName]) {
        console.log(`  Removing ${cyan(ownPackageName)} from devDependencies`);
        delete appPackage.devDependencies[ownPackageName];
      }
    }
    appPackage.dependencies = appPackage.dependencies || {};
    if (appPackage.dependencies[ownPackageName]) {
      console.log(`  Removing ${cyan(ownPackageName)} from dependencies`);
      delete appPackage.dependencies[ownPackageName];
    }
```



把ownPackage中的依赖(dependencies)都拷贝到appPackage中去

```
Object.keys(ownPackage.dependencies).forEach(key => {
      // For some reason optionalDependencies end up in dependencies after install
      if (ownPackage.optionalDependencies[key]) {
        return;
      }
      console.log(`  Adding ${cyan(key)} to dependencies`);
      appPackage.dependencies[key] = ownPackage.dependencies[key];
    });
```



最终命令行中执行打印如下:

```
Updating the dependencies
  Removing react-scripts from dependencies
  Adding @babel/core to dependencies
  Adding @svgr/webpack to dependencies
  Adding @typescript-eslint/eslint-plugin to dependencies
  Adding @typescript-eslint/parser to dependencies
  Adding babel-eslint to dependencies
  Adding babel-jest to dependencies
  Adding babel-loader to dependencies
  Adding babel-plugin-named-asset-import to dependencies
  Adding babel-preset-react-app to dependencies
  Adding camelcase to dependencies
  Adding case-sensitive-paths-webpack-plugin to dependencies
  Adding css-loader to dependencies
  Adding dotenv to dependencies
  Adding dotenv-expand to dependencies
  Adding eslint to dependencies
  Adding eslint-config-react-app to dependencies
  Adding eslint-loader to dependencies
  Adding eslint-plugin-flowtype to dependencies
  Adding eslint-plugin-import to dependencies
  Adding eslint-plugin-jsx-a11y to dependencies
  Adding eslint-plugin-react to dependencies
  Adding eslint-plugin-react-hooks to dependencies
  Adding file-loader to dependencies
  Adding fs-extra to dependencies
  Adding html-webpack-plugin to dependencies
  Adding identity-obj-proxy to dependencies
  Adding is-wsl to dependencies
  Adding jest to dependencies
  Adding jest-environment-jsdom-fourteen to dependencies
  Adding jest-resolve to dependencies
  Adding jest-watch-typeahead to dependencies
  Adding mini-css-extract-plugin to dependencies
  Adding optimize-css-assets-webpack-plugin to dependencies
  Adding pnp-webpack-plugin to dependencies
  Adding postcss-flexbugs-fixes to dependencies
  Adding postcss-loader to dependencies
  Adding postcss-normalize to dependencies
  Adding postcss-preset-env to dependencies
  Adding postcss-safe-parser to dependencies
  Adding react-app-polyfill to dependencies
  Adding react-dev-utils to dependencies
  Adding resolve to dependencies
  Adding resolve-url-loader to dependencies
  Adding sass-loader to dependencies
  Adding semver to dependencies
  Adding style-loader to dependencies
  Adding terser-webpack-plugin to dependencies
  Adding ts-pnp to dependencies
  Adding url-loader to dependencies
  Adding webpack to dependencies
  Adding webpack-dev-server to dependencies
  Adding webpack-manifest-plugin to dependencies
  Adding workbox-webpack-plugin to dependencies
```



顺便还要给依赖的名称做个排序，溜得不行

```
const unsortedDependencies = appPackage.dependencies;
    appPackage.dependencies = {};
    Object.keys(unsortedDependencies)
      .sort()
      .forEach(key => {
        appPackage.dependencies[key] = unsortedDependencies[key];
      });
```



更新script命令，删除scripts中的eject命令

```
delete appPackage.scripts['eject'];
```

从

```
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
 }
 ==>
 
 "scripts": {
    "start": "node scripts/start.js",
    "build": "node scripts/build.js",
    "test": "node scripts/test.js"
  }
```





添加eslint, babel, jtest等的配置

```
// Add Jest config
    console.log(`  Adding ${cyan('Jest')} configuration`);
    appPackage.jest = jestConfig;

    // Add Babel config
    console.log(`  Adding ${cyan('Babel')} preset`);
    appPackage.babel = {
      presets: ['react-app'],
    };

    // Add ESlint config
    console.log(`  Adding ${cyan('ESLint')} configuration`);
    appPackage.eslintConfig = {
      extends: 'react-app',
    };

    fs.writeFileSync(
      path.join(appPath, 'package.json'),
      JSON.stringify(appPackage, null, 2) + os.EOL
    );
    console.log();
    

Configuring package.json
  Adding Jest configuration
  Adding Babel preset
```



在做一些配置后，执行yarn或者npm命令，把依赖包都安装起来。