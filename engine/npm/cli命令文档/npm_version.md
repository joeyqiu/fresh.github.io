## npm version

Bump a package version

Run this in a package directory to bump the version and write the new data back to `package.json`, `package-lock.json`, and, if present, `npm-shrinkwrap.json`.

The `newversion` argument should be a valid semver string, a valid second argument to [semver.inc](https://github.com/npm/node-semver#functions) (one of `patch`, `minor`, `major`, `prepatch`, `preminor`, `premajor`, `prerelease`), or `from-git`. In the second case, the existing version will be incremented by 1 in the specified field. `from-git` will try to read the latest git tag, and use that as the new npm version.

If run in a git repo, it will also create a version commit and tag. This behavior is controlled by `git-tag-version` (see below), and can be disabled on the command line by running `npm --no-git-tag-version version`. It will fail if the working directory is not clean, unless the `-f` or `--force` flag is set.

碰撞一个版本号，会根据你传入的参数，更新补丁版本、小版本、主要版本好，如果是在git仓库中，也是自动创建一个commit和tag命令，还能接受指定的commit信息。

