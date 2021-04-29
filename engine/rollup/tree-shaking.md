## tree-shaking

rollup打包后的工具库，放在webpack项目中引用，是自动支持tree-shaking的。



但是使用方式需要注意下，

### 库

库里面，工具代码需要通过`export` 方式，一个个导出不同的方法，这样才能按需载入。



### 使用项目中

```
import {getBabelChannel, testTree} from '@zero/jdyfe-common-util'
```

这边两个方法引入之后，getBabelChannel` 方法在项目中使用到了，然后`testTree` 方法没有使用。然后看打包后的代码，发现`testTree` 的代码也能够智能的不载入，真正做到了按需加载，就很棒。





打包压缩后，多余的代码被clear掉了？只要在peerDependencies中就行