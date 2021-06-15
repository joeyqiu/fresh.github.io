## git pull 遇到error

在执行git pull时遇到如下错误：

```shell
error: cannot lock ref 'xxx': ref xxx is at （一个commitID） but expected
```

遇到这个错误会导致git pull失败。

### 问题原因

原因是你这个git工程的.git/refs目录下跟踪的某些git分支，在git pull的时候，与远端的对应分支的refs对比发现不同，所以导致git pull报错。

通常产生这个问题的原因是（以分支git/yousa/feature_01为例）：

1. 有人操作git/yousa/feature_01这个分支，在git push的时候使用了git push –force，（当然这个人的git push是push不上去），导致远端分支被覆盖，你本地的refs与远端无法一致，导致问题
2. git分支是不区分大小写的，如果有人删除掉这个远端分支又重新新建了一个这个分支也会出现同样的问题。



### 解决办法

根据前面的原因有一些操作方法，基本思路就是要么，强行git pull；要么则是删除掉有问题的refs，再进行git pull（个人还是推荐第二种）（以分支git/yousa/feature_01为例）

1. 删除有问题的refs，可以直接在.git/refs下面根据错误提示删除对应的refs文件，比如这个就是需要删除refs/remotes/origin/git/yousa/feature_01文件（嫌麻烦直接删除整个refs目录也行）
2. 使用git命令删除相应refs文件，`git update-ref -d refs/remotes/origin/git/yousa/feature_01`
3. 简单粗暴强行git pull，执行`git pull -p`



### 另一种思路

```
error: cannot lock ref 'xxx': ref xxx is at （一个commitID
```

直观得看错误信息，git本次操作是获取锁失败，git的锁是使用一个共享的文件锁，故删除对应的index.lock文件也可以解决该问题。（直接释放锁）