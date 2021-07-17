官网：https://classic.yarnpkg.com/en/docs/install#mac-stable 按照官网的介绍，使用brew进行安装，然后也想使用brew进行更新，但是总是提示失败：

```
➜  drip-design git:(dev) brew upgrade yarn     
Error: yarn 1.3.2 already installed
```

总是提示1.3.2版本已经安装，但是其实是想更新到1.12的版本。

后来按照stackoverflow上的解决方法：https://stackoverflow.com/questions/51837889/why-doesnt-yarn-updated-when-running-upgrade

先卸载yarn，然后通过curl来安装。

First, uninstall brew's yarn:

```
brew uninstall yarn
```

Removing yarn binaries manually:

```
rm -f /usr/local/bin/yarnpkg
rm -f /usr/local/bin/yarn
```

Remove yarn cache:

```
rm -rf ${HOME}/.yarn
```

If you have the following in your .zshrc or .bash_profile, remove it:

```
export PATH="$PATH:`yarn global bin`"
```

Install via curl:

```
curl -o- -L https://yarnpkg.com/install.sh | bash
```

Make sure there is the following line in your .zshrc or .bash_profile:

```
export PATH="$HOME/.yarn/bin:$PATH"
```