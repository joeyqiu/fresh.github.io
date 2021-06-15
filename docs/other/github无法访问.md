# github可以ping通，但是无法访问

今天打开github页面的时候，突然就一直无法打开了，但是试了下ping通是可以的。后来百度查找各种方案，是下来设置host的方式是可以解决这个问题的。



## 添加host

1. 准备工作，打开本地电脑配置的host，查看是否配置`github.com`映射域名

Mac电脑：

```
/etc/hosts
```

如果装了iHost或者switchHost等工具的话，就不需要这么麻烦了。



2. 查找可用配置并修改

获取github.com在天朝内可用的dns域名，打开： `http://tool.chinaz.com/dns?type=1&host=www.github.com&ip=` 网站，获取TTL最小的值：

![](./imgs/github_host.png)

然后把这个ip配置到host中保存就可以了。



```
13.259.177.223 github.com
```

