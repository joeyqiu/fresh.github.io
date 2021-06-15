## git多人协作


当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。


````

$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)

````
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

### 推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，git就会把该分支推送到远程库对应的远程分支上

````
git push origin master
````
如果需要推送其它分支，比如dev，则：

````
git push origin dev
````	


### 抓取分支
多人协作时，大家都会忘master和dev分支上推送各自的修改。

如果小伙伴clone了master分支，然后想在dev分支上修改，则直接创建远程origin的dev分支到本地：

````
git checkout -b dev origin/dev
````

现在，小伙伴可以在dev上继续修改，然后时不时把dev分支push到远程:

````
git commit -m "xxx"
git push origin dev
````

可能，你也同时修改了dev分支，并修改了相同的文件，并试图推送：

````
$ git add hello.py 
$ git commit -m "add coding: utf-8"
[dev bd6ae48] add coding: utf-8
 1 file changed, 1 insertion(+)
$ git push origin dev
To git@github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
````

发现推送失败，因为文件之间有冲突。。。所以需要先本地`git pull`把最新的提交从origin/dev抓下来，然后与本地合并，解决冲突，再推送：

````
$ git pull
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (3/3), done.
From github.com:michaelliao/learngit
   fc38031..291bea8  dev        -> origin/dev
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream dev origin/<branch>
````

如果发现`git pull`也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

````
$ git branch --set-upstream dev origin/dev
Branch dev set up to track remote branch dev from origin.
````
在pull:

````
$ git pull
Auto-merging hello.py
CONFLICT (content): Merge conflict in hello.py
Automatic merge failed; fix conflicts and then commit the result.
````
pull成功之后，在本地解决合并冲突，在push上去。


### 总结
多人协作的工作模式通常是这样的：

1. 试图用`git push origin branch-name`推送自己的修改
2. 如果推送失败，则因为远程分支比你的本地更新，所以先用`git pull`试图合并
3. 如果合并有冲突，则解决冲突，并在本地提交
4. 没有冲突或者解决之后，再用`git push origin branch-name`推送就能成功

如果`git pull`提示“no tracking information”，说明本地分支和远程分支的链接关系没有建立，用命令`git branch --set-upstream branch-name origin/branch-name`建立连接。














