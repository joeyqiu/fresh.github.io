chrome版本：版本 84.0.4147.105（正式版本） （64 位）

请在chrome的地址栏输入：

```
chrome://net-internals/#hsts
```

在打开的页面中， Delete domain 栏的输入框中输入：xx.xx.com（注意这里是二级域名），然后点击“delete”按钮，即可完成配置。

然后你可以在 Query domain 栏中搜索刚才输入的域名，点击“query”按钮后如果提示“Not found”，那么你现在就可以使用http来访问了！

