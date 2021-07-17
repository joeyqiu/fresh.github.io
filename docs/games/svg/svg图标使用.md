开发过程中，很多小图标，现在都是通过矢量图标svg来展示了。

当设计把svg给到之后，可以把所有需要使用的svg都列在html中，然后容器进行隐藏

```
<div hidden="">
      <svg xmlns="http://www.w3.org/2000/svg">
        <symbol id="icon-star" viewBox="0 0 14 13">
          <path
            d="M7.395.296l1.486 2.93c.22.436.648.738 1.141.808l3.322.47a.528.528 0 0 1 .303.906l-2.404 2.28c-.357.34-.52.828-.436 1.307l.567 3.22c.077.435-.391.766-.792.561l-2.97-1.52a1.553 1.553 0 0 0-1.412 0l-2.97 1.52c-.4.205-.869-.126-.792-.56l.567-3.22A1.452 1.452 0 0 0 2.57 7.69L.165 5.41a.528.528 0 0 1 .303-.906l3.321-.47c.494-.07.921-.372 1.142-.808L6.417.296a.553.553 0 0 1 .978 0"
          ></path>
        </symbol>
      </svg>
    </div>
```

使用的时候直接用如下方式就行

```
<svg className="card-star" aria-hidden="true">
    <use xlinkHref="#icon-star" />
</svg>
```

通过symbol上设置id，然后使用的时候，通过xlinkHref(xlink:href，前面是react中的写法)来引用。

可以给不同的svg设置不同的颜色，然后发现颜色不生效，可能是fill属性没有设置。

```
svg {
  fill: currentColor;
}
```

参考网址：[m.qidian.com](http://m.qidian.com)

### 设置

![设置](https://img14.360buyimg.com/imagetools/jfs/t1/150302/37/15876/103295/5fbf72d4E594f1007/a934c5699d84d6d9.png?ynotemdtimestamp=1626536525406)

### 使用

![使用](https://img10.360buyimg.com/imagetools/jfs/t1/146198/4/16039/73648/5fbf7349E9b9eb69b/52e348d620c23158.png?ynotemdtimestamp=1626536525406)