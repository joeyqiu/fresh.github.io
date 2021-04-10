## X-Requested-With引起的跨域问题

用AntD的Uploader组件来上传图片或者文件的时候，会在`request-headers`属性中自动添加`X-Requested-With`配置。

加了这个配置后，需要接口做出对应的配置，否则还是会导致跨域失败。



### 现象

错误提示如下：

```
has been blocked by CORS policy: Request header field x-request-with is not allowed by Access-Control-Allow-Headers in preflight response.
```



### 解读

上传图片都是post请求，post请求之前都有一个preflight的预请求，用来看接口是否支持后续的post请求。

这边提示`Access-Control-Allow-Headers` 不支持 `x-request-with` ，所以导致的CORS。



### 解决方法

这次遇到问题，主要是接口的nginx配置的`Access-Control-Allow-Headers` 不支持 `x-request-with`，所以让接口修改下nginx配置。然后看下修改后的response headers

```
access-control-allow-headers: Content-Type, x-requested-with

access-control-allow-origin: http://mcbeta.jd.com
```

就能正确解决了，，因为是有跨域设置的接口，如果allow-origin:* 就没问题了。



### 延伸

**x-requested-with 请求头 区分ajax请求还是普通请求**

 Ajax 请求相比普通请求会多个 x-requested-with ，可以利用它，request.getHeader(“x-requested-with”); 为 null，则为传统同步请求，为 XMLHttpRequest，则为 Ajax 异步请求。

