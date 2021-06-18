## 跨域资源共享CORS

一个域请求另外一个域的资源，就会发生资源跨域的问题。



跨域资源共享（CORS）是一种机制，它使用额外的HTTP头来告诉浏览器，让运行在一个origin（域）上的web应用被准许访问来自不同服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源就会发起一个跨域HTTP请求。

比如，站点` http://domain-a.com` 的某 HTML 页面通过`<img/>`标签的src属性，去请求`http://domain-b.com/image.jpg`。网络上的许多页面都会加载来自不同域的CSS样式表，图像和脚本等资源。

__出于安全原因，浏览器限制从脚本内发起的跨域HTTP请求。__例如，XMLHttpRequest和Fetch API遵循同源策略。所以除非服务端对该域名做了允许访问的处理，否则会爆出跨域访问的错误。



### 为什么img标签没有跨域错误

浏览器可以加载跨域资源，但是因为安全原因，对脚本内的跨域有限制，但是img标签是在html中的，所以没有该限制。

```
The cross-origin sharing standard does not include img tags, but XHR/fetch requests and some cases including drawing images to a canvas does.
```





跨域资源共享标准新增了一组HTTP首部字段，允许服务器声明哪些源站通过浏览器有权限访问那些资源。



`Access-Control-Allow-Origin` 应当为 * 或者包含由 Origin 首部字段所指明的域名。

Origin是指请求头中包含的origin，也就是当前域，一个简单的请求头如下：

```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
Origin: http://foo.example
```

