## html标签上的lang属性

The lang attribute(in no namespace) specifies the primary language for the element's contents and for any of the element's attributes that contain text. It's value must be a valid BCP 47 language tag, or the empty string. empty string indicates that the primary language is unknown.

lang全局属性是一个字符串类型的语言标签。如果标签的内容是空字符串，语言就设为未知。或者如果标签内容是无效的，根据BCP47，它就设为无效。

参考链接： https://tools.ietf.org/html/bcp47



单一的zh和zh-CN现在都已经被废弃了。



简体中文页面：html lang=zh-cmn-Hans
繁体中文页面：html lang=zh-cmn-Hant
英语页面：html lang=en



问题主要在于，zh 现在不是语言code了，而是macrolang，能作为语言code的是cmn（国语）、yue（粤语）、wuu（吴语）等。我通常建议写成 zh-cmn 而不是光写 cmn，主要是考虑兼容性（至少可匹配 zh），有不少软件和框架还没有据此更新。

zh-CN 的问题还在于，其实多数情况下标记的是简体中文，但是不恰当的使用了地区，这导致同样用简体中文的 zh-SG（新加坡）等无法匹配。更典型的是 zh-TW 和 zh-HK。所以其实应该使用 zh-Hans / zh-Hant 来表示简体和繁体。那么完整的写法就是 zh-cmn-Hans，表示简体中文书写的普通话/国语。一般而言没有必要加地区代码，除非要表示地区特异性，一般是词汇不一样（比如维基百科的大陆简体和新马简体）。



__如果不需要考虑到这么多的地区，其实lang="zh-CN"就可以了__

