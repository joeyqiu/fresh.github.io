## KOA路由



在启动脚本中index.js

```
var Koa=require('koa');  
var app=new Koa();  
var router = require('./routes');

app.use(router.routes());
app.listen(3000);
```



在route.js文件中

```
const router = require('koa-router')();

router.get('/',async (ctx)=>{  
    ctx.body="首页";  
});
  
router.get('/news',async (ctx)=>{  
    ctx.body="新闻列表页面";  
});

//动态路由  http://localhost:3002/newscontent/xxxx  
router.get('/newscontent/:aid',async (ctx)=>{
    //获取动态路由的传值  
    console.log(ctx.params);  //{ aid: '456' }  
    ctx.body="新闻详情";  
});

//动态路由里面可以传入多个值    
//http://localhost:3002/package/123/456  
router.get('/package/:aid/:cid',async (ctx)=>{
    //获取动态路由的传值  
    console.log(ctx.params);  //{ aid: '123', cid: '456' }  
    ctx.body="新闻详情";
});
```

