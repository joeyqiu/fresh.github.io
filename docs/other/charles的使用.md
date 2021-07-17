使用的是4.2.6版本的charles。 https://www.charlesproxy.com/download/latest-release/

一般手机联调的时候，只需要在同一个局域网内，就可以通过在手机端浏览器上打开电脑上启动的服务了。但有时候情况有些特殊，比如需要在app内打开页面，然后app又无法打开localhost页面，就可以使用charles开进行帮助调试。

### charles的配置

先下载安装charles，然后确保电脑和手机是链接的同一个局域网。 启动charles后，需要进行证书的安装

#### 电脑上证书的安装

```
Help => SSL Proxying => Install Charles Root Certificate
```

#### 手机上证书的安装

```
Help => SSL Proxying => Install Charles Root Certificate on a Mobile Device or Remote Browser
```

然后会有一个弹窗提示，一般都是需要配置手机上连接的wifi，代理到电脑上的ip地址和端口，然后通过`chls.pro/ssl`网址来下载安装证书，然后再进入手机的证书页面进行启动信任证书。

端口可以在charles的`proxy => proxy settings => Http Proxies` 中进行指定。

就这样，电脑上和手机上的证书都安装好，然后手机上指向了电脑上的ip和端口，一切都准备好了。

### 在app内调试指定页面

使用`Tools => Map Remote` 功能， 在app内打开指定页面，然后在Map Remote中设置如下:

Map from：app中打开的原页面，想通过该页面映射为本地服务的页面

```
Protocol: https
Host: 域名(比如app中打开的页面是m.baidu.com)
port: 默认都是443端口
```

Map To: 本地页面

```
Protocol: http (本地协议)
Host: dev.m.com (127.0.0.1映射的本地域名，随意写)
port: 10086 (本地服务启动的端口)
```

这样一映射之后，打开`https:m.baidu.com`下的所有页面，都会映射到`http://dev.m.com:10086` 去。 如果不想映射这么多的话，还有path和，制定路径来映射。

### map local功能用来映射接口返回