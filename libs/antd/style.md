## AntDesign - style

antd中，每个组件的style里面都有个`index.tsx`文件，通过js的方式来引入样式代码。

所以在使用antd的时候，需要写引入样式部分的代码：

```
@import '~antd/dist/antd.css';
```

或者通过`babel-plugin-import`插件来实现自动引入样式代码。



### 组件中

组件中的style/index.tsx文件，基本都是如下，引入公用style目录和组件的index.less。

```
import '../../style/index.less';
import './index.less';
```

少部分组件会依赖其余组件，所以也需要把组件的样式也引入，比如slider组件中，就依赖了tooltip组件，所以slider的style中引入了tooltip的样式。

```
import '../../style/index.less';
import './index.less';

// style dependencies
import '../../tooltip/style';
```



### style

```

```

