https://www.npmjs.com/package/husky

husky是的git钩子实现的更简单。钩子可以在进行git提交和添加的时候进行规定的检查，避免一些不正规的代码提交。

Husky can prevent bad git commit, git push and more 🐶 *woof!*

husky支持git文档中定义的所有钩子(hook)：https://git-scm.com/docs/githooks

安装

```
npm install husky --save-dev
```



配置，在package.json中

```
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm test",
      "pre-push": "npm test",
      "...": "..."
    }
  }
}
```



hooks是保存在hooks目录的程序，通过指定的git的行为进行触发。默认情况下，hooks目录是在`$GIT_DIR/hooks`，也就是项目的`.git/hooks`下面，当然目录也是可以修改的。



查看hooks目录，可以看到如下所有的hooks程序：

- applypatch-msg
- applypatch-msg.sample(简单的demo程序)
- commit-msg
- commit-msg.sample
- famonitor-watchman.sample
- post-applypatch
- post-checkout
- post-commit
- post-merge
- post-receive
- post-rewrite
- post-update
- post-update.sample
- pre-applypatch
- pre-applypatch.sample
- pre-auto-gc
- pre-commit
- pre-commit.msg
- pre-merge-commit
- pre-push
- pre-push.sample
- pre-rebase
- pre-rebase.sample
- pre-receive
- pre-receive.sample
- prepare-commit-msg
- prepare-commit-msg.sample
- push-to-checkout
- sendemail-validate
- update
- update.sample

pre表示行为之前，post表示行为之后。



**commit-msg**

这个hook可以被`git-commit`和`git-merge`调用，也可以通过传递`--no-verify`选项来避免调用。