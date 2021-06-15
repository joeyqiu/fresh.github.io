# MongoDB 删除数据库

MongoDB 删除数据库的语法格式如下：

```sql
db.dropDatabase()
```

删除当前数据库，默认为 test，你可以使用 db 命令查看当前数据库名。

### 实例

以下实例我们删除了数据库 books。

首先，查看所有数据库：

```
> show dbs
local   0.078GB
books  0.078GB
test    0.078GB
```

接下来我们切换到数据库 books：

```
> use books
switched to db books
> 
```

执行删除命令：

```
> db.dropDatabase()
{ "dropped" : "books", "ok" : 1 }
```

最后，我们再通过 show dbs 命令数据库是否删除成功：

```
> show dbs
local  0.078GB
test   0.078GB
> 
```

 