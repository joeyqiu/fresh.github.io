
## cat 命令

通过cat命令读文件的时候，通过grep来过滤

````
cat wifiNearby_act.log.20171207 | grep '432033092dce434cb9d73239de4b0195'

先过滤source，然后过滤scheme，然后过滤小时，最后打印过滤重复

cat wifiNearby_act.log.20171016 |awk '{print $1,$4}'|sort -n|uniq -c|awk '{print $3}' |sort |uniq -c

cat wifiNearby_act.log.20170810 | grep '"source":"2"' | grep 'scheme":"C' | grep '2017-08-10 20' | awk '{print $1,$4}'|sort -n|uniq -c|awk '{print $3}' |sort |uniq -c
````

## npm设置淘宝源

````
通过淘宝源下载文件

镜像使用方法（三种办法任意一种都能解决问题，建议使用第三种，将配置写死，下次用的时候配置还在）:

1.通过config命令
npm config set registry https://registry.npm.taobao.org 
npm info underscore （如果上面配置正确这个命令会有字符串response）

2.命令行指定
npm --registry https://registry.npm.taobao.org info underscore 

3.编辑 ~/.npmrc 加入下面内容
registry = https://registry.npm.taobao.org
````

## ssh/scp命令

````
登陆新的测试环境服务器(先上跳板机，在登陆)
ssh -p 58422 server@58.215.44.42
ssh root@10.136.24.75

上传到跳板机：
scp -P 58422 webroot.tar.gz server@58.215.44.42:~/

从跳板机上传到测试环境
scp -p webzx-1.2.0-20170630-1622-test.tar.gz root@10.136.24.75:~/

````

## 添加软连接
````
ln -s 添加软连接

ln -s 源文件目录 目标
````

## mac上添加环境变量

````
通过.bash_profile来修改

1，vim ~/.bash_profile
进入后看到export PATH=$PATH:/xx/xx

把路径用:隔开，放在最后。

2，cat ~/.bash_profile, 修改完毕， :wq退出，执行source ~/.bash_profile。

3，echo $PATH，查看路径是否添加成功。

以冒号分隔
export NGINX_HOME=/usr/local/Cellar/nginx/1.12.2_1
export PATH=$NGINX_HOME/bin:$JRE_HOME/bin:$PATH  
````

## 手机远程调试
````
android4.4及以上，可以使用chrome的远程调试
chrome://inspect

以下，通过adblogcat命令调试，
adb logcat | grep Web
````

## mac下生成rsa key
````
ssh-keygen -t rsa -q -P "" -C "#####qiujinming#2016-06-21" -f "id_rsa"

如果上面的命令没效果，则执行下面的试试
ssh-keygen -t rsa 
````

## tar压缩与解压缩

````
tar [-cxtzjvfpPN] 文件与目录 ....
参数：
-c ：建立一个压缩文件的参数指令(create 的意思)；
-x ：解开一个压缩文件的参数指令！
-t ：查看 tarfile 里面的文件！ 特别注意，在参数的下达中， c/x/t 仅能存在一个！不可同时存在！ 因为不可能同时压缩与解压缩。
 -z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩？
范例一：将整个 /etc 目录下的文件全部打包成为 /tmp/etc.tar
[root@linux ~]# tar -cvf /tmp/etc.tar /etc         <==仅打包，不压缩
[root@linux ~]# tar -zcvf /tmp/etc.tar.gz /etc       <==打包后，以 gzip 压缩

范例二：查阅上述 /tmp/etc.tar.gz 文件内有哪些文件？ [root@linux ~]# tar -ztvf /tmp/etc.tar.gz
由於我们使用 gzip 压缩，所以要查阅该 tar file 内的文件时，
就得要加上 z 这个参数了！这很重要的！
 
范例三：将 /tmp/etc.tar.gz 文件解压缩在 /usr/local/src 底下
[root@linux ~]# cd /usr/local/src
[root@linux src]# tar -zxvf /tmp/etc.tar.gz


压缩：
tar -zcvf dist.tar.gz dist

解压缩：
tar -zxvf dist.tar.gz
````