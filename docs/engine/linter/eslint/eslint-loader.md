## eslint-loader

eslint在webpack中的配置使用

First, run the linter. It's important to do this before Babel processes the JS. 需要在babel处理之前先去进行语法检查。

```
{
          test: /\.(js|mjs|jsx|ts|tsx)$/,
          enforce: 'pre',
          use: [
            {
              options: {
                eslintPath: require.resolve('eslint'),
                emitWarning: true,
                useEslintrc: true
              },
              loader: require.resolve('eslint-loader'),
            },
          ],
          include: paths.appSrc,
        },
```

安全起见，就需要设置`enforce: 'pre'` 来优先检查源代码。



### options

Note that the config option you provide will be passed to the `CLIEngine`. This is a different set of options than what you'd specified in `package.json` or `.eslintrc.` 

options对象会传递给eslint的`CLIEngine`, 然后这个`CLIEngine`的文档参考： https://eslint.org/docs/developer-guide/nodejs-api#cliengine



### useEslintrc: true

Set tot false to disable use of `.eslintrc` files(default: true), Corresponds to `--no-eslintrc`

觉得默认的eslint-laoder中，没有使用eslintrc的配置文件，所以需要在options中在设置一下，然后采用eslintrc中的配置。



### emitWarning: true

如果语法中有不合格的时候，通过设置`emitWarning: true` 来通过warning的方式报出，防止默认的阻塞操作。



### cache

如果在配置的修改过程中，不能使用cache，否则eslintrc中的改动会无法采用到，反而是之前缓存的配置。所以需要慎用。