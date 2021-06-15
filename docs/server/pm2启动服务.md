# pm2启动服务

通过pm2在后台启动进程服务



启动命令

````
pm2 start index.js --name "blog"
````


显示所有进程

````
pm2 list
````

相关命令

````
pm2 stop     <app_name|id|'all'|json_conf>
pm2 restart  <app_name|id|'all'|json_conf>
pm2 delete   <app_name|id|'all'|json_conf>
````
