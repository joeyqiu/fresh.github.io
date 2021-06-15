## KOA获取get传值



在koa的get请求中，如何获取传递的参数



加入请求地址为：`http://localhost:3002/newscontent?aid=123` 

```
//获取get传值 
router.get('/newscontent',async (ctx)=>{  
    /*在 koa2 中 GET 传值通过 request 接收，但是接收的方法有两种：query 和 querystring。 
     query：返回的是格式化好的参数对象。 
     querystring：返回的是请求字符串。*/  
  
    //从ctx中读取get传值  
  
    console.log(ctx.query);  //{ aid: '123' }       获取的是对象   用的最多的方式      ******推荐  
  
    console.log(ctx.querystring);  //aid=123&name=zhangsan      获取的是一个字符串  
  
    console.log(ctx.url);   //获取url地址  
  
    //ctx里面的request里面获取get传值  
  
    console.log(ctx.request.url);  
    console.log(ctx.request.query);   //{ aid: '123', name: 'zhangsan' }  对象  
    console.log(ctx.request.querystring);   //aid=123&name=zhangsan  
  
  
    ctx.body="新闻详情";  
  
})  
```



