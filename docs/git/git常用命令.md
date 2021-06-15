# Git常用命令列表

* [Create](#Create)
* [配置git](#配置git)
* [文件版本操作](#文件版本操作)
* [Commit History](#Commit_History)
* [版本分支](#版本分支)
* [标签](#标签)
* [Update & Publish](#Update&Publish)
* [Merge and Rebase](#Merge&Rebase)
* [UNDO](#UNDO)
* [others](#others)



### Create

```
# clone一个已经存在的仓库
$ git clone ssh://xxx or https://xxx

# 创建一个本地的仓库，初始化，创建一个.git文件
$ git init
```



## 配置git

```
# 检查已有配置信息
$ git config --list
# 配置信息设置
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com" 
$ git config --global color.ui "always"
```



## 文件版本操作

```
# changed files in your working directory
$ git status
# 查看当前目录下所有有改动的文件
$ git status . 

# changes to tracked files
$ git diff

# 添加
$ git add somefile.txt  # 添加单个文件到本地版本库
$ git add *.txt         # 添加所有的txt文件到本地版本库

# 添加所有的子目录（不包含空目录）到本地版本库
# 会把工作时的所有变化提交到暂存区，包括文件的修改以及新文件，但不包括被删除的文件
$ git add .             

# 会把工作时修改的文件提交到暂存区，但仅包括已经被存在的文件（tracked file，包括被删除的文件）
$ git add -u

# 所有变化文件都提交到暂存区
$ git add -A

# 提交
$ git commit -m "msg" somefile.txt                # 提交 单个文件
$ git commit -m "msg" -a                          # 提交 所有修改文件
$ git commit -C head -a -amend                    # 增补提交，不会产生新的提交历史

# change the last commit，指修改本地的commit信息，改不到pushed到网上的。
git commit --amend

# 撤销未提交的文件
$ git checkout head a.txt b.txt     # 撤销单个文件
$ git checkout head *.txt           # 撤销所有txt文件
$ git checkout head .               # 撤销所有文件
$ git checkout --file : 丢弃工作区的修改。
# 撤销已提交的文件
$ git revert --no-commit head <filename>      # 撤销最近一次的提交
```



## Commit_History

```
# show all commits, starting with newest
$ git log

# show changes over time for a specific file
$ git log -p <file>

# who changed what and when in <file>
$ git blame <file>
```



## 版本分支

```
$ git branch                # 列出本地分支
$ git branch -a             # 列出本地所有分支
$ git branch -av             # 列出所有分支
$ git branch -r                     # 列出所有远程库分支

$ git checkout <branchName>       # 签出分支, switch head branch

$ git branch <branchName>         # 基于当前分支创建新的分支，但并不签出，所以不在新创建的分枝上
$ git checkout -b <branchName>    # 基于当前分支创建新的分支并签出
$ git checkout -b mybranch origin/abranch #将创建mybranch和跟踪origin/abranch
 
$ git branch -m <branchName> <newName>  # 不会覆盖已存在的同名分支
$ git branch -M <branchName> <newName>  # 会覆盖已存在的同名分支
 
$ git branch -d <newName>                 # 如果分支未合并会删除失败
$ git branch -D <newName>                 # 强制删除分支
 
$ git branch -r -d origin/<branchName>       #删除远程分支1
$ git push origin :<branchName>              #删除远程分支2
 
$ git remote prune origin           # 删除远程库不存在的分支
$ git merge –no–ff <branchName>       # 快速合并分支
```

## 标签

```
$ git tag                               # 显示所有标签列表
 
$ git tag <tagName>                     # 当前最后一次提交后的分支上创建标签
$ git tag <tagName> <branchName>        # 为特定分支最后一次提交后的状态创建标签
$ git tag <tagName> <version>           # 为历史版本提交创建标签
 
$ git checkout <tagName>                # 签出标签（快速查看基于某个标签下的断面，但不能提交）
 
$ git tag -d <tagName>   # 删除标签

切换个分支来合并tag代码。
$ git checkout destination_branch
$ git merge tag_name

# publish your tags
$ git push --tags
```



## Update&Publish

```
$ ssh-keygen -t rsa -C "youremail@example.com"    #cat ~/.ssh/id_rsa.pub 上传公钥
 
# list all currently configured remote
$ git remote -v

# show information about a remote
$ git remote show <remote>

#添加远程版本库origin
$ git remote add <origin> <URL> 

#更新指定远程版本库origin但不合并, (在多个远程仓库的情况下，比较有用)
$ git fetch <origin>

#获取并合并到本地分支
$ git pull = git pull <origin> 
# download changes and directly merge/integrate into HEAD,拉去远程合并到本地
$ git pull <remote> <branch>
# publish local changes on a remote
$ git push <remote> <branch>  

修改远程分支地址：
git remote set-url origin  git@git.palmchat.corp:server/help-web.git
```



### Merge&Rebase

```
# merge <branch> into your current HEAD
$ git merge <branch>

# rebase your current HEAD onto <branch>, don't rebase published commits
$ git rebase <branch>
rebase只适合修改一个人的仓库，且是本地的，如果是多人协作，不要用。

# abort a rebase
$ git rebase --abort

# cotinue a rebase after resolving conflits
$ git rebase --continue

# use your configured merge tool to solve conflits
$ git mergetool

# use your editor to manually solve conflits and(after resolving) mark files as resolved
$ git add <resolved-file>
$ git rm <resolved-file>
```



### UNDO

```
# discard all local changes in your workding directory
$ git reset --hard HEAD

# discard local changes in a specific file
$ git checkout HEAD <file>

# revert a commit (by producing a new commit with contrary changes)
# 撤销指定的版本，撤销也会作为一次提交进行保存
$ git revert <commit>

# reset your head pointer to a previois commit and discard all changes since then
$ git reset --hard <commit>
# and preserve all changes as unstaged changes
$ git reset <commit>
# and preserve uncommited local changes
$ git reset --keep <commit>

#查看命令历史，以便确定要回到未来的哪个版本
$ git reflog	   
如果是比较严重的操作错误，需要恢复到某个版本，使用`git reflog`查看历史版本，然后根据版本号和reset来回退到之前的版本
```



## others

```
/.gitignore     #忽略特殊文件，添加到版库中，也支持版本管理
#简化命令行配置
$ git config --global alias.st status       #输入git st = git status;   其他命令同理

## 清理之前保存的用户名/密码（如果之前输入的是错误的）
git config --system --unset credential.helper

# 查看帮助手册
$ git help
$ git help <command>
```