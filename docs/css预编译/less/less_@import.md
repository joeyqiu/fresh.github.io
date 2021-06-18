## less的@import

Less中，可以通过 @import指令来导入外部文件。@import指令可以放在代码中的任何位置，导入文件时的处理方式取决于文件的扩展名：

- 如果扩展名是 .css，文件内容将被原样输出。
- 如果是任何别的扩展名，将作为LESS文件被导入。
- 如果没有扩展名，将会添加一个 .less 的扩展名，并作为LESS文件被导入。

例如：

1. `@import "style";   // 导入 style.less`
2. `@import "style.less"; // 导入style.less`
3. `@import "style.php";  //  style.php 作为LESS文件被导入`
4. `@import "style.css";  // 文件内容被原样输出`

一个网站常常是有多个模块组成，如果只使用一个 .less 文件，编辑起来非常不便，也不利于分工协作。此时，就可以为每个模块单独创建 .less 文件，然后通过 @import指令将它们合并成一个文件。

假如一个网站包含产品、新闻、BBS三个模块，就可以为每个模块单独创建一个 .less 文件，分别是 product.less、news.less、bbs.less。然后，在 style.less 中，通过 @import指令将它们合并成一个文件：

1. `@import "product.less";`
2. `@import "news.less";`
3. `@import "bbs.less";`

导入外部文件的一个常见应用场景，就是变量共享。通常是在一个 .less 文件中定义一些变量，其他文件只需导入这个 .less 文件，就可以使用这些变量。如，在 base.less 中定义 @color 变量：

1. `@color: #fff;`

然后，在 styles.less 文件中，只需使用 @import指令导入base.less 文件，就可以使用它的变量 @color，而不必重复定义。代码如下：

```
@import "base.less";

.myclass {
	background-color: @color;
}
```



styles.less 编译后的CSS代码为：

```
.myclass {
	background-color: #fff;
}
```



另外，为了在将Less文件编译生成CSS文件时，提高对外部文件的操作灵活性，还为@import指令提供了一些配置项。语法为：

1. `@import (keyword) "filename";`

@import指令的配置项及其含义如下：

| 选项      | 含义                                                 |
| --------- | ---------------------------------------------------- |
| reference | 使用文件，但不会输出其内容（即，文件作为样式库使用） |
| inline    | 对文件的内容不作任何处理，直接输出                   |
| less      | 无论文件的扩展名是什么，都将作为LESS文件被输出       |
| css       | 无论文件的扩展名是什么，都将作为CSS文件被输出        |
| once      | 文件仅被导入一次 （这也是默认行为）                  |
| multiple  | 文件可以被导入多次                                   |
| optional  | 当文件不存在时，继续编译（即，该文件是可选的）       |

一个@import指令可以使用一个或多个配置项，当使用多个配置项时，各配置项之间用逗号隔开。如：

`@import (optional, reference) "foo.less";`

