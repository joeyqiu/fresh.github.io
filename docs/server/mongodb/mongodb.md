# MongoDB

### 启动
安装之后通过如下方式启动服务

````
1. 首先我们创建一个数据库存储目录 /data/db：
sudo mkdir -p /data/db

2. sudo mongod

3. 再打开一个终端进入执行以下命令：./mongo(对mongo的目录下，通常为/usr/local/mongodb/bin )

在./mongo命令后开启mongo数据库，可以执行各种操作
````




### 后台启动以及关闭



创建配置文件

````
# vi /data/db/mongodb.cnf

dbpath=/data/db/
logpath=/data/db/mongo.log
logappend=true
fork=true
port=27017
````



配置文件方式启动mongo

```
# mongod -f /data/db/mongodb.cnf 

# mongod -f /data/db/mongodb.cnf & （放到后台执行）
```



### 关闭

```
通过kill命令关闭
查看进程： ps -ef | grep mongo
关闭进程： kill xxx


mongod --shutdown在我电脑上无效，不知道为啥
```

