## config-provider

我应该需要补一下React的context和Hook相关的知识了，说实话，并不是很看得懂这个文件。

#### Button中的使用

```
import { ConfigContext } from '../config-provider';
import SizeContext, { SizeType } from '../config-provider/SizeContext';

const size = React.useContext(SizeContext);
// 使用hook的写法，来得到sizeContent中配置的size值

const { getPrefixCls, autoInsertSpaceInButton, direction } = React.useContext(ConfigContext);
// getPrefixCls是一个函数，用来获取前缀
// autoInsertSpaceInButton是一个布尔值，是否在Button的文案之间自动插入空格
// direction,表示文案的方向。
```

sizeContext提供size相关的值，剩下的可以在ConfigContext中获取。











