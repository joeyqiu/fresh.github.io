## export 'ButtonProps' (reexported as 'ButtonProps') was not found in './button'

```
WARNING in ../components/button/index.tsx 2:0-64
export 'ButtonProps' (reexported as 'ButtonProps') was not found in './button' (possible exports: convertLegacyProps, default)
```

在本地跑服务的时候发现，当export的是type或者interface的时候，就会出现上面的各种错误信息。



参考链接：

* https://github.com/babel/babel-loader/issues/603

* https://github.com/TypeStrong/ts-loader/issues/751

