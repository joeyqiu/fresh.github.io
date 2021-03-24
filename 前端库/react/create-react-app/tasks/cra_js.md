# cra.js

packagesDir是packages的目录地址。说明这边的文件系统会遍历packages目录下的所有npm包

```
const rootDir = path.join(__dirname, '..');
// packages目录和tasks目录是同级的
const packagesDir = path.join(rootDir, 'packages');
const packagePathsByName = {};

fs.readdirSync(packagesDir).forEach(name => {
  const packageDir = path.join(packagesDir, name);
  const packageJson = path.join(packageDir, 'package.json');
  
  // 得到文件夹下的package.json文件
  
  if (fs.existsSync(packageJson)) {
  // 如果有了package.json文件，后续才需要去包里面遍历
    packagePathsByName[name] = packageDir;
  }
});
```



修改本地的依赖路径，dependencies, devDependencies, peerDependencies, optionalDependencies四种依赖。

```
// 遍历packages目录下，所有文件夹中有package.json配置文件的目录
Object.keys(packagePathsByName).forEach(name => {
  const packageJson = path.join(packagePathsByName[name], 'package.json');
  // 读取package.json文件，并解析成utf8格式的json对象
  const json = JSON.parse(fs.readFileSync(packageJson, 'utf8'));
  
  // 嵌套遍历
  Object.keys(packagePathsByName).forEach(otherName => {
  	// 如果packages.json中依赖的正好在当前目录，直接本地修改依赖路径
    if (json.dependencies && json.dependencies[otherName]) {
      json.dependencies[otherName] = 'file:' + packagePathsByName[otherName];
    }
    if (json.devDependencies && json.devDependencies[otherName]) {
      json.devDependencies[otherName] = 'file:' + packagePathsByName[otherName];
    }
    if (json.peerDependencies && json.peerDependencies[otherName]) {
      json.peerDependencies[otherName] =
        'file:' + packagePathsByName[otherName];
    }
    if (json.optionalDependencies && json.optionalDependencies[otherName]) {
      json.optionalDependencies[otherName] =
        'file:' + packagePathsByName[otherName];
    }
  });
	
	// 重新写入修改后的package.json文件内容
  fs.writeFileSync(packageJson, JSON.stringify(json, null, 2), 'utf8');
  console.log(
    'Replaced local dependencies in packages/' + name + '/package.json'
  );
});
```





最终做了一些准备后，开始执行脚本，就是执行 `create-react-app/index.js`

````
const craScriptPath = path.join(packagesDir, 'create-react-app', 'index.js');
cp.execSync(
  `node ${craScriptPath} ${args.join(' ')} --scripts-version="${scriptsPath}"`,
  {
    cwd: rootDir,
    stdio: 'inherit',
  }
);
````

