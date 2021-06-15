## git生成patch

通过git修改后，如果需要提供patch给别人，可以通过如下方式获取：

````
1、 git log 查看修改
````

每个commit信息后面都有id

````
2、 git format-patch id(commentId，截取前10位就行)
````

哦，这边的id，需要的是，修改的前一个id，如果有三次提交，，你需要第一次后面的两次修改，这边用的id就需要时第一次的



应用patch有两种命令。

````
3、git apply --check newpatch.patch 
````

git apply并不会将commit message等打上去，打完patch后需要重新git add和git commit





````
4、git am newpatch.patch
````

git am会直接将patch的所有信息打上去，而且不用重新git add和git commit,author也是patch的author而不是打patch的人