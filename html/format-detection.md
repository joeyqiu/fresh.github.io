# Meta: format-detection

### meta标签

`<meta>`元素可提供有关页面的元信息（meta-information）, 比如针对搜索引擎和更新频度的描述和关键词。

`<meta>`标签永远位于head元素的头部，在html中没有结束标签。

必需的属性是：content。

可选的属性是：http-equiv、name、scehme。



参考链接：

* [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name)
* [https://wiki.whatwg.org/wiki/MetaExtensions](https://wiki.whatwg.org/wiki/MetaExtensions)



### format-detection

format-detection —— 格式检测，用来检测html里的一些格式，主要有以下几个设置： 

* name=”format-detection” content=”telephone=no”
* name=”format-detection” content=”email=no”
* name=”format-detection” content=”adress=no”



或者直接写成

* name=”format-detection” content=”telephone=no,email=no,adress=no”



### 常见设置

```
<meta name="format-detection" content="telephone=no">
```





### telephone 
主要作用是是否设置自动将你的数字转化为拨号连接 
telephone=no 禁止把数字转化为拨号链接 
telephone=yes 开启把数字转化为拨号链接，默认开启

### email 
告诉设备不识别邮箱，点击之后不自动发送 
email=no 禁止作为邮箱地址 
email=yes 开启把文字默认为邮箱地址，默认情况开启

### adress 
adress=no 禁止跳转至地图 
