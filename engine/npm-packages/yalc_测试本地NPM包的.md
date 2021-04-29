## yalc

参考链接：https://www.npmjs.com/package/yalc

这边摘取部分翻译



```
Better workflow than npm | yarn link for package authors.
```

对于npm/yarn包的作者来说，yalc工具提供了一个更好的工作流程。



### 为什么而用

当npm包的作者在开发的时候，你经常会发现需要在本地环境中使用正在开发中的包，而且这个包还不能上传到远程仓库中去。就或者比如你开发完之后，想要本地验证下， 总不可能每次验证都传递到仓库中去哦，因为每次传递都需要修改package.json中的version信息。

NPM和Yarn提供的一个类似的方法，如使用软连接，`npm/yarn link` ， 但是好像经常会因为依赖而导致一些限制和问题？



### yalc是什么

* yalc为本地开发的包，提供了一个本地的仓库，所以你可以在本地环境中进行分享
* 当开包的开发目录中执行`yalc publish` 命令的时候，yalc也是仅仅把那些会传递到npm/yarn仓库的代码(所以.npmignore配置也是有效的哦)传递到本地仓库中去，一个特殊的全局存储空间（例如~/.yalc）
* 当你在使用包的开发项目中执行`yalc add package` 的时候，会直接把包的内容放置在当前目录的`.yalc` 文件夹，然后在`package.json` 中插入一条`file:/link` 的依赖。或者你也可以通过`yalc link package` 来创建一个`node_modules` 到包内容的符号链接，然后link命令不会改变`package.json` （yalc link 命令和 npm/yarn link 命令差不多）
* yalc会在你的使用包的开发项目中创建一个`yalc.lock` 文件 （类似于yarn.lock, package-lock.json），用来确保保持一致性。
* yalc可以在项目中和npm还有yarn命令搭配使用



### 安装

```
npm i yalc -g
```

or

```
yarn global add yalc
```

应该是必须要全局安装的



### 使用教程

#### publish

* 在你的`package` （也就是本地开发的npm包目录中）目录中执行`yarn publish` 命令
* It will copy [all the files that should be published in remote NPM registry](https://docs.npmjs.com/files/package.json#files).
* If your package has one of these lifecycle scripts: `preyalc`, `prepare`, `prepack`, `prepublishOnly`, `prepublish`, it will run before. If your package has one of these: `postyalc` or `postpublish`, it will run after. Use `--force` to publish without running scripts.
* 当拷贝包内的时候，yalc会计算所有文件的hash签名，然后把改签名添加到包的version配置中，可以通过`--no-sig` 选项来取消
* You may also use `.yalcignore` to exclude files from publishing to yalc repo, for example, files like README.md, etc.
* `--files` flag will show included files in the published package
* **NB!** In terms of which files will be included in the package `yalc` fully supposed to emulate behavior of `npm` client (`npm pack`). [If you have nested `.yalc` folder in your package](https://github.com/whitecolor/yalc/issues/95) that you are going to publish with `yalc` and you use `package.json` `files` list, it should be included there explicitly.
* **NB!** Windows users should make sure the `LF` new line symbol is used in published sources; it may be needed for some packages to work correctly (for example, `bin` scripts). `yalc` won't convert line endings for you (because `npm` and `yarn` won't either).
* **NB!** Note that, if you want to include `.yalc` folder in published package content, you should add `!.yalc` line to `.npmignore`.



#### Add

* 在需要本地包的开发项目中执行`yalc add package` 命令来安装，该命令会拷贝指定版本的包内容到项目的`.yalc` 目录，并在`package.json` 中插入一个`file:/.yalc/package` 的依赖声明.
* 还能通过`yalc add package@version` 来指定版本。然后这个版本会在`yalc.lock` 文件中固定，并且不会影响新版本的更新。
* Use the `--link` option to add a `link:` dependency instead of `file:`.
* 使用`--dev` 选项用来添加yalc包到开发依赖中
* With `--pure` flag it will not touch `package.json` file, nor it will touch modules folder, this is useful for example when working with [**Yarn workspaces**](https://yarnpkg.com/lang/en/docs/workspaces/) (read below in *Advanced usage* section)



#### Link

* 除了add命令，当然还可以用link命令
* As an alternative to `add`, you can use the `link` command which is similar to `npm/yarn link`, except that the symlink source will be not the global link directory but the local `.yalc` folder of your project.
* After `yalc` copies package content to `.yalc` folder it will create a symlink: `project/.yalc/my-package ==> project/node_modules/my-package`. It will not touch `package.json` in this case.



#### Update

* Run `yalc update my-package` to update the latest version from store.
* Run `yalc update` to update all the packages found in `yalc.lock`.
* While update if yalc'ed package has `scripts.postupdate` this command will run in host package dir.



#### Remove

- Run `yalc remove my-package`, it will remove package info from `package.json` and `yalc.lock`
- Run `yalc remove --all` to remove all packages from project.



#### Installations

- Run `yalc installations clean my-package` to unpublish a package published with `yalc publish`
- Run `yalc installations show my-package` to show all packages to which `my-package` has been installed.

这两个命令展示了如何下架和展示所有yalc发布的模块





## Advanced usage

### Pushing updates automatically to all installations

- When you run `yalc add|link|update`, the project's package locations are tracked and saved, so `yalc` knows where each package in the store is being used in your local environment.

- `yalc publish --push` will publish your package to the store and propagate all changes to existing `yalc` package installations (this will actually do `update` operation on the location).

- ```
  yalc push
  ```

   

  \- is a use shortcut command for push operation (which will likely become your primarily used command for publication):

  - `force` options is `true` by default, so it won't run `pre/post` scripts (may change this with `--no-force` flag).

- `scripts.postupdate` will be executed in host package dir, like while `update` operation.

- With `--changed` flag yalc will first check if package content has changed before publishing and pushing, it is a quick operation and may be useful for *file watching scenarios* with pushing on changes.

### Keep it out of git

- If you are using `yalc'ed` modules temporarily during development, first add `.yalc` and `yalc.lock` to `.gitignore`.
- Use `yalc link`, that won't touch `package.json`
- If you use `yalc add` it will change `package.json`, and ads `file:`/`link:` dependencies, if you may want to use `yalc check` in the [precommit hook](https://github.com/typicode/husky) which will check package.json for `yalc'ed` dependencies and exits with an error if you forgot to remove them.

### Keep it in git

- You may want to keep shared `yalc'ed` stuff within the projects you are working on and treat it as a part of the project's codebase. This may really simplify management and usage of shared *work in progress* packages within your projects and help to make things consistent. So, then just do it, keep `.yalc` folder and `yalc.lock` in git.
- Replace it with published versions from remote repository when ready.
- **NB!** - standard non-code files like `README`, `LICENCE` etc. will be included also, so you may want to exclude them in `.gitignore` with a line like `**/.yalc/**/*.md` or you may use `.yalcignore` not to include those files in package content.

### Publish/push sub-projects

- Useful for monorepos (projects with multiple sub-projects/packages): `yalc publish some-project` will perform publish operation in the `./some-project` directory relative to `process.cwd()`

### Use with [**Yarn workspaces**](https://yarnpkg.com/lang/en/docs/workspaces/)!

Use if you will try to `add` repo in `workspaces` enabled package, `--pure` option will be used by default, so `package.json` and modules folder will not be touched.

Then you add yalc'ed package folder to `workspaces` in `package.json` (you may just add `.yalc/*` and `.yalc/@*/*` patterns). While `update` (or `push`) operation, packages content will be updated automatically and `yarn` will care about everything else.

If you want to override default pure behavior use `--no-pure` flag.

### Clean up installations file

- While working with yalc for some time on the dev machine you may face the situation when you have locations where you added yalc'ed packages being removed from file system, and this will cause some warning messages when yalc will try to push package to removed location. To get rid of such messages, there is an explicit command for this: `yalc installations clean [package]`.

### Override default package store folder

- You may use `--store-folder` flag option to override default location for storing published packages.







