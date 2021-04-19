## 一些vscode配置问题

* [vscode配置中文](#vscode配置中文)
* [写js代码报ts的错误信息](#写js代码报ts的错误信息)
* [vscode折叠设置](#vscode折叠设置)
* [Typescript扩展中可以关闭vscode的自动提示功能](#Typescript扩展中可以关闭vscode的自动提示功能)



### vscode配置中文

* View => Command Palette
* 在输入框中输入：configure Display language
* 看到有"Install additional languages..."，点击这个
* 然后会让你选择安装的语言，选择中文插件，然后重启即可.



### 写js代码报ts的错误信息

在js文件中，总会有ts的错误提示，，网上搜索一番，然后应该还是ts插件配置的问题。（应该都是新版本）

打开vscode的配置，在typescript配置中，查找 Javascript > Validate: Enabled，如果勾选上的话，取消勾选就可以了。



### vscode折叠设置

vscode中折叠了代码后，有时候展开的icon就不展示了，就导致无法查看折叠起来的代码，就比较的尴尬。

Code =》 首选项 =〉设置，输入`foldingStrategy`，然后不要用auto，选择`indentation`即可。



### Typescript扩展中可以关闭vscode的自动提示功能

Code => 首选项 => 用户 => 扩展 => Typescript,

JavaScript > Suggest: Enabled。

某天不小心关闭这个选择框之后，vscode代码的自动提示功能就不生效了。

