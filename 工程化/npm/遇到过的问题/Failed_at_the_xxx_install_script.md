## peer dependency and it's warning

npm安装的时候会遇到如下错误信息

```

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! iltorb@2.4.5 install: `node ./scripts/install.js || node-gyp rebuild`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the iltorb@2.4.5 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
```



failed at the xx install scripts。 所以使用 `--ignore-scripts` 参数先忽略script

```
npm install xxx --ignore-scripts
```







