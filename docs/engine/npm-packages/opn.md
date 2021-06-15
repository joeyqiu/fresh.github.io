## opn

文档地址：https://www.npmjs.com/package/opn



a better `node-open`. Opens stuff like websites, files, executables, cross-platform.



### Usage

```
const opn = require('opn');
 
// Opens the image in the default image viewer
opn('unicorn.png').then(() => {
    // image viewer closed
});
 
// Opens the url in the default browser
opn('http://sindresorhus.com');
 
// Specify the app to open in
opn('http://sindresorhus.com', {app: 'firefox'});
 
// Specify app arguments
opn('http://sindresorhus.com', {app: ['google chrome', '--incognito']});
// --incognito: 隐身模式？

```





Uses the command `open` on macOS, `start` on Windows and `xdg-open` on other platforms.



### 参数app

##### app

Type: `string` `Array`

Specify the app to open the `target` with, or an array with the app and app arguments.

The app name is platform dependent. Don't hard code it in reusable modules. For example, Chrome is `google chrome` on macOS, `google-chrome` on Linux and `chrome` on Windows