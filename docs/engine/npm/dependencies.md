## 各种NPM依赖的介绍



### Dependencies

```
Dependencies are listed in the package.json file in a dependencies object.

When you add a package in dependencies, you are saying:

* My code needs this package to run.
* If this package doesn’t already exist in my node_modules directory, then add it automatically.
* Furthermore, add the packages that are listed in the package’s dependencies. These packages are called transitive dependencies.
```



### peerDependencies

某些情况下，想要表达工具或者库与自己包的兼容性问题，

```
Peer Dependencies are listed in the package.json file in a peerDependencies object.

By adding a package in peerDependencies you are saying:

* My code is compatible with this version of the package.
* If this package already exists in node_modules, do nothing.
* If this package doesn’t already exist in the node_modules directory or it is the wrong version, don’t add it. But, show a warning to the user that it wasn’t found.
```

