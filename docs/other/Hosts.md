## hosts

#### 1 什么是Hosts

首先，我们知道网络中通过DNS服务来解析域名与IP地址之间的对应关系 

在我们本地的操作系统上，实际上也存在着一个与DNS起到相同作用的东西：Hosts文件 

当用户在浏览器中输入一个需要登录的网址时 

系统会首先自动从[Hosts文件](https://baike.baidu.com/item/Hosts文件)中寻找对应的[IP地址](https://baike.baidu.com/item/IP地址) 

一旦找到，系统会立即打开对应网页 

如果没有找到，则系统会再将网址提交DNS[域名解析](https://baike.baidu.com/item/域名解析)服务器进行IP地址的解析

由此得知，Hosts文件承担了一部分解析域名与IP的功能，并且优先级高于DNS服务 

#### 2 为什么要使用Hosts

DNS通常架设在互联网上，任何人只要知道该网站的域名，就可以访问到这个网站 

但是某些开发环境，我们不需要暴露在整个互联网上，仅仅开发人员自己可以访问就可以了 

所以此时，使用Hosts文件的优越性就凸显出来了 

因为Hosts以本地文件的形式存储在我们的操作系统中，只有Hosts文件配置正确，才能通过域名访问到指定网站 



#### 3 如何使用Hosts文件 

不同操作系统将Hosts文件存储到了不同的目录下 ：

```
Win：C:\windows\system32\drivers\etc\ hosts
Linux：/etc/ hosts
Mac ：/private/etc/hosts
Android：/system/etc/ hosts
```

Hosts文件是一个没有拓展名的文本文件，使用记事本就可以打开 ，语法格式如下：

```
# 注释
IP地址 域名
 
# 例如：
172.17.54.101 jd.com
```

以上的配置，设置了[jd.com](http://jd.com/)这个域名对应的IP地址为172.17.54.101 

当我们访问这个域名时，系统会根据Hosts文件中的配置去到指定的ip地址 

我们使用的开发环境多数都可以以这种方式访问 
