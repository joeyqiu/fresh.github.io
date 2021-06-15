## 输出

`output`位于对象最顶级键，包括了一组选项，指示webpack如何去输出，以及在哪里输出你的bundle, asset和别的你所打包或者使用webpack载入的任何内容。



### path

output目录对应一个绝对路径



### pathinfo

告知webpack再bundle中引入“所包含模块信息”的相关注释。

Add /* filename */ comments to generated require()s in the output.



### filename

决定了每个输出bundle的名称。这些bundle将写入到`output.path`选项指定的目录下。

可用替换模版字符串

| --          | 描述                                             |
| ----------- | ------------------------------------------------ |
| [hash]      | 模块标识符(module identifier)的 hash             |
| [chunkhash] | chunk 内容的 hash                                |
| [name]      | 模块名称                                         |
| [id]        | 模块标识符(module identifier)                    |
| [query]     | 模块的 query，例如，文件名 `?` 后面的字符串      |
| [function]  | The function, which can return filename [string] |



### chunkFilename

决定了非入口(non-entry)chunk文件的名称。

这些文件名需要在runtime根据chunk发送的请求去生成。



### publicPath

对于按需加载（on-demand-load）或加载外部资源（external resources）(图片，文件等)来说，publicPath是很重要的选项。如果指定了一个错误的值，加载这些资源会收到404错误。

此选项指定在浏览器中所引用的“此输出目录对应的公开URL”。相对URL会被相对于HTML页面解析。相对于服务的URL，相对于协议的URL或绝对URL也可是可能用到的，或者又是必须用到。例如：当将资源托管到CDN时。

该选项的值是以runtime（运行时）或loader（载入时）所创建的每个URL为前缀。因此多数情况下， 此选项的值都会以`/`结束。





### devtoolModuleFilenameTemplate

自定义每个`source map`的`sources`数组中使用的名称。可以通过传递模版字符串或者函数来完成。