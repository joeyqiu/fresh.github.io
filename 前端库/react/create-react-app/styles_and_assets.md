## Styles and Assets



### Adding Images, Fonts, and Files

to reduce the number of requests to the server, importing images that are less than __10000 bytes(9.765625 KB)__ returns a  data URI instead of a path. This applies to the following file extensions: bmp, gif, jpg, jpeg, and png.

svg也可以通过webpack来引入，但是需要别的方法.



### Using the Public Folder

the `public` folder contains the HTML files so you can tweak it, for example, to set the page title.

The `<script>` tag with the compiled code will be added to it automatically during the build process.



#### Adding Assets Outside of the Module System

外部资源如何添加，最好是通过js中的import来动态导入。

如果需要在index.html中加载外部资源，还是需要把资源文件移动`public`目录，然后通过`PUBLIC_URL`来引入，否则public中的文件夹，即使build的时候打包到了dist目录，也不会被引入html中。



Inside `index.html`，用如下方式引入public中的其它文件。

````
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
````

only files inside the `public` folder will be accessible by `%PUBLIC_URL%`prefix.

当运行`npm run build`命令的时候， 会用正确的地址来替换`%PUBLIC_URL%`。



#### When to Use the public folder

尽量使用js来导入别的资源。

Normally we recommend importing stylesheets, images, and fonts from Javascript. The `public` folder is useful as a workaround for a number of less common case:

- You need a file with a specific name in the build output, such as [`manifest.webmanifest`](https://developer.mozilla.org/en-US/docs/Web/Manifest).
- You have thousands of images and need to dynamically reference their paths.
- You want to include a small script like [`pace.js`](http://github.hubspot.com/pace/docs/welcome/) outside of the bundled code.
- Some library may be incompatible with Webpack and you have no other option but to include it as a `<script>` tag.



### Code Splitting

Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small chunks which you can then load on demands.

