tree shaking是一个术语，通用用于描述移除Javascript上下文中未引用代码（dead-code）。依赖于es2015模块语法的静态结构特性：import 和 export。这个术语和概念是由rollup普及起来的。

看下rollup官网关于什么是tree-shaking的回答：

```
Tree-shaking, also known as "live code inclusion", is Rollup's process of eliminating code that is not actually used in a given project. It is a form of dead code elimination but can be much more efficient than other approaches with regard to output size.
The name is derived from the abstract syntax tree(抽象语法树) of the module(not the module graph). the algorithm first mark all relevant statements(标记所有相关语句) and then "shaking the syntax tree" to remove all dead code. It's similar in idea to the [mark-and-sweep garbage collection algorithm](https://en.wikipedia.org/wiki/Tracing_garbage_collection). Event though this algorithm is not restricted to ES module, they make it much more efficient as they allow Rollup to treat all modules together as a big abstract syntax tree with shared bindings.
```

tree-shaking依赖于esmodule的语法

所以可以看下rollup官网回答的关于为什么ES模块比CommonJS更好？

```
ES模块是官方标准，也是JS语言明确发展的方向，而CommonJS模块是一种特殊的传统格式，在ES模块被提出之前作为暂时的解决方案。ES模块允许进行静态分析，从而实现像tree-shaking的优化和作用域提升， 并提供诸如循环引用和 动态绑定等高级功能。
helps with optimizations like tree-shaking and scope-hoisting, and provide advanced features like cicular references and living binding.
```

## rollup中的treeshaking

先看下rollup官网关于treeshaking似乎不起效果的一个回答：

```
sometimes, you'll end up with code in your bundle that doesn't seem like should be there. For example, if you import a utility from `loadsh-es`, you might expect that you'll get the bare minimum(最少量) of code necessary for that utility to work.

But rollup has to be conservative(保守的) about what code it removes in order to guarantee that the end result will run correctly. If an imported module appears to have side-effects, either on bits of the module that you're using or on the global environment, Rollup plays it safe and includes those side-effects.

because static analysis in a dynamic language like Javascript is hard, there will occasionally be false positives. Loadsh is a good example of a module that looks like it has lots of side-effects, even in places that it doesn't. you can offen mitigate(减缓、减轻) those false positive(假的正确) by importing submodules(e.g. import map from 'lash-es/map' rather than import {map} from 'lashsh-es').

rollup's static analysis will improve over time. but is will never be perfect in all cases - that's just javascript.
```

rollup在删除代码方便会比较的保守，一切都是为了确保最终的打包结果是正确的。如果引入的模块是有side-effects函数的，rollup就会把模块和side-effects函数一起打包进来。

### named exports

这边假设打包出来都只看esmodule格式的文件内容

#### export一个对象，一个属性。

```
demo1.js

const something = true;
export { something };

// 后者
export const something = true; // 都一样的效果
```

引用的入口文件

```
import { something } from './demo1.js';
```

这边只是应用，但是不调用，所以最终打包出来是一片空白.

```
import { something } from './demo1.js';
console.log(something);
```

这边引入并且使用了，打包出来的esmodule文件内容如下：

```
var something = true;

console.log(something);
```

#### export一个对象，多个属性。

```
demo1.js

const something = true;
const again = true;
export { something, again };
import { something, again } from './demo1.js';
```

虽然都引用了，但是没有调用，所以是一片空白。

```
import { something, again } from './demo1.js';
console.log(again);
```

打包出来和之前一样，只有使用到的被打包进去了。

```
var again = true;

console.log(again);
```

### Default Export

```
Only expressions, functions or classes are allowed as the `default` export
```

export a single value as the source module's default export, is only recommend if your source module only has one export.

**it's a bad partice to mix default and named exports in the same module, 尽管这是被规则允许的，但Rollup依然认为这是一个坏习惯。**

```
demo1.js

const something = true;
const again = false;

export default {something}
```

引入文件

```
import demo from './demo1.js';
console.log(demo);
```

最终打包出来

```
var something = true;
var demo = {
  something: something
};

console.log(demo);
```

export default导出的是一个默认对象，然后这边引入的时候命名为demo,

```
demo1.js
const something = true;
const again = false;

export default {something, again}
import demo from './demo1.js';
console.log(demo);
```

打包出来

```
var something = true;
var again = false;
var demo = {
  something: something,
  again: again
};

console.log(demo);
```

因为打印的是整个导出的对象，所以还是把所有属性都打印出来了。

基于上述原因，即使只是用了其中的一个方法，也会把整个对象打印出来.

```
import demo from './demo1.js';
console.log(demo.something);
```

打包出来：

```
var something = true;
var again = false;
var demo = {
  something: something,
  again: again
};

console.log(demo.something);
```

如果引入之后也是没有使用，default的变量也不会被打包进来.

```
import demo from './demo1.js';
```

打包后还是一片空白。

### named export 混合 default export

虽然不是个好习惯，但是还是有存在的情况。

```
const something = true;
const again = false;

export const badValue = 'bad';

export default {something, again}
```

引入

```
import * as demo from './demo1.js';
```

因为没有使用，所以一片空白

```
import * as demo from './demo1.js';
console.log(demo);
```

打包出来可以看到所有属性都带上了。

```
var something = true;
var again = false;
var badValue = 'bad';
var demo1 = {
  something: something,
  again: again
};

var demo = /*#__PURE__*/Object.freeze({
	__proto__: null,
	badValue: badValue,
	'default': demo1
});

console.log(demo);
```

只使用named export值:

```
import {badValue} from './demo1.js';
console.log(badValue);
```

打包后

```
var badValue = 'bad';

console.log(badValue);
```

只是用default export值：

```
import {default as haha} from './demo1.js';
console.log(haha);
```

打包后，就相当于导出对象的default属性。这边使用的时候给default改了个haha的名字。

```
var something = true;
var again = false;
var haha = {
  something: something,
  again: again
};

console.log(haha);
```

#### 含有side-effect的函数

```
demo1.js

export function writeName(){
	const name = getName();
	return name;
}

function getName() {
	return 'joey';
}
import {writeName} from './demo1.js';
writeName()
```

writeName()只是返回name值，所以不算使用，所以打包后还是一片空白。

```
import {writeName} from './demo1.js';
console.log(writeName())
```

通过console函数，就算是使用了，最终打包的时候会把side-effects函数getName带进来。

```
function writeName() {
  var name = getName();
  return name;
}

function getName() {
  return 'joey';
}

console.log(writeName());
```

### 结论

rollup中的代码遵守ESModule的情况下， tree-shaking都比较的顺利，当然那些副作用的函数还没看。

### webpack中的tree-shaking

webapck2正式版内置支持ES2015模块（也叫做harmony modules） 和未使用模块的检测能力。新的webacpk4正式版扩展了此检测能力，通过`package.json`的`sideEffects`属性作为标记，向compiler提供提示，标明项目中哪些文件是`pure(纯的ES2015模块)`，由此可以安全的删除文件中未使用的部分。

webpack现在也是支持tree-shaking的，，但是webpack主要是用在项目中居多，然后项目会使用到很多第三方以来的库。 因为tree-shaking只在esmodule的规范下支持，如果第三方库没有提供esmodule的文件，那么就无法使用treeshaking功能。

比如：

```
import VConsole from 'vconsole';
```

打包之后项目中依然还有该vconsole，就是因为引用的是umd规范的文件。所以这个时候就体现出module字段的重要性了，第三方库打包的时候需要提供module字段来进行tree-shaking。