## gzip-size

文档地址：https://www.npmjs.com/package/gzip-size



Get the gzipped size of a string or buffer



### Usage

```
const gzipSize = require('gzip-size');
const text = 'Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.';
 
console.log(text.length);
//=> 191
 
console.log(gzipSize.sync(text));
//=> 78
```

