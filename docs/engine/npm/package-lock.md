## package-lock

package-lock.json is automitically generated for any operations where npm modifies either the node_modules tree, or package.json. It describes the exact tree that was generated, such that subsequent installs are able to generate identical trees, regardless of intermediate dependency updates.

package-lock.json文件是自动生成的，描述了生成的准确树，随后的安装都能生成一个完全相同的树，不管内部依赖项如何更新。(依赖更新会自动更新package-lock.json文件) 所以package-lock.json是用来生成一个完全相同的依赖树。

This file is intended to be committed into source repositories, and serves various purposes:

- Describe a single representation of a dependency tree such that teammates, deployments, and continuous integration are guaranteed to install exactly the same dependencies.
- Provide a facility for users to “time-travel” to previous states of node_modules without having to commit the directory itself.
- To facilitate greater visibility of tree changes through readable source control diffs.
- And optimize the installation process by allowing npm to skip repeated metadata resolutions for previously-installed packages.

One key detail about package-lock.json is that it cannot be published, and it will be ignored if found in any place other than the toplevel package.

### 推荐使用

package-lock.json文件需要提交到git源里面去，但是一个关键点是不能给发布到npm包去，为什么？

提交到git源去，是因为在不同的机器上，安装相同的依赖树。

但是不提交到npm包里面？是npm包只需要包含打包后的文件，并不需要去开发，所以传入package-lock.json文件也是不需要的。

### 使用场景

项目push到远端之后，带有package.json和package-lock.json两个文件，package.json指定了版本， package-lock.json指定整个依赖树。

试验一下，package.json和package-lock.json中，指定不同的版本，最终项目会安装成什么样的版本呢？ 比如一个包：`uglifyjs-webpack-plugin`

| package.json | package-lock.json | 安装的版本                                           |
| ------------ | ----------------- | ---------------------------------------------------- |
| 2.0.0        | 2.0.0             | 2.0.0                                                |
| 2.2.0        | 2.0.0             | 2.2.0（并且把package-lock.json中的2.0.0更新成2.2.0） |
| 1.3.0        | 2.0.0             | 1.3.0 (并且把package-lock.json中的2.0.0更新成1.3.0)  |

结果:

- 安装的时候会以package.json中的版本为主，且更新package-lock.json中对应包的版本

现在把`uglifyjs-webpack-plugin`依赖包`webpack-sources`，在package-lock.json中，版本从1.4.3 => 0.2.3，有两个地方有版本指定，一个是包声明，一个是`uglifyjs-webpack-plugin`的依赖字段中指定的版本，看以哪个版本为主。

```
"uglifyjs-webpack-plugin": {
  "version": "1.3.0",
  "resolved": "http://registry.m.jd.com/uglifyjs-webpack-plugin/download/uglifyjs-webpack-plugin-1.3.0.tgz",
  "integrity": "sha1-dfVIFghYFjoIZD4IbV/v4YpdZ94=",
  "dev": true,
  "requires": {
    "cacache": "^10.0.4",
    "find-cache-dir": "^1.0.0",
    "schema-utils": "^0.4.5",
    "serialize-javascript": "^1.4.0",
    "source-map": "^0.6.1",
    "uglify-es": "^3.3.4",
    "webpack-sources": "^1.1.0",
    "worker-farm": "^1.5.2"
  }
},
"webpack-sources": {
  "version": "1.4.3",
  "resolved": "http://registry.m.jd.com/webpack-sources/download/webpack-sources-1.4.3.tgz",
  "integrity": "sha1-7t2OwLko+/HL/plOItLYkPMwqTM=",
  "dev": true,
  "requires": {
    "source-list-map": "^2.0.0",
    "source-map": "~0.6.1"
  }
},
```

===>

```
"uglifyjs-webpack-plugin": {
  "version": "1.3.0",
  "resolved": "http://registry.m.jd.com/uglifyjs-webpack-plugin/download/uglifyjs-webpack-plugin-1.3.0.tgz",
  "integrity": "sha1-dfVIFghYFjoIZD4IbV/v4YpdZ94=",
  "dev": true,
  "requires": {
    "cacache": "^10.0.4",
    "find-cache-dir": "^1.0.0",
    "schema-utils": "^0.4.5",
    "serialize-javascript": "^1.4.0",
    "source-map": "^0.6.1",
    "uglify-es": "^3.3.4",
    "webpack-sources": "^1.1.0",
    "worker-farm": "^1.5.2"
  },
  "dependencies": {
    "webpack-sources": {
      "version": "1.4.3",
      "resolved": "http://registry.m.jd.com/webpack-sources/download/webpack-sources-1.4.3.tgz",
      "integrity": "sha1-7t2OwLko+/HL/plOItLYkPMwqTM=",
      "dev": true,
      "requires": {
        "source-list-map": "^2.0.0",
        "source-map": "~0.6.1"
      }
    }
  }
},
```

为什么我修改下版本好，直接依赖就直接修改位子了，从外层的node_modules移到 `uglifyjs-webpack-plugin` 的内部node_moduels中去了。难道是版本不同就会这样的改动？然后版本还是以`uglifyjs-webpack-plugin`这边依赖中指定的版本为主。所以其实都是package.json中的版本为主。 package-lock.json只是一个依赖树的展示。

### 总结

- 安装包只看项目的package.json中指定的包版本，package-lock.json只是一个依赖展示树
- 不过可以手动修改package-lock.json中项目包的依赖包版本，就相当于修改项目包的package.json，这样会修改安装的项目包的依赖包版本
