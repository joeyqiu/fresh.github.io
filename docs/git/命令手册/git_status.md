## git status

show the working tree status.

展示工作区域的状态



### 简介(Synopsis)

```
git status [<options>…​] [--] [<pathspec>…​]
```



### Description



### Options

````
--porcelain[=<version>]
用一种脚本容易解析的方式来输出。类似简写的输出，但在不同的git版本中能够保持稳定，并且忽略用户的配置。
version字段用于制定输出的版本，是可选的，并且默认是v1的版本格式。

Give the output in an easy-to-parse format for scripts. This is similar to the short ouput, but will remain stable across Git versions and regardless of user configuration. 

the version parameter is used to specify the format version, is optional and defaults to the original version v1 format.
````

