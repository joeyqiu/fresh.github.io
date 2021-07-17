## canvas.toDataUrl跨域报错

当使用canvas绘制网络图片的时候，经常会出现“Tainted canvases may not be exported”报错，上网搜一下解决方案，应该给的都是给img添加crossOrigin属性，尝试了一下，确实可用。

```
const img = document.createElement('img');
	    img.crossOrigin = 'anonymous';
	    img.onload=function(){
	    	ctx.drawImage(img, 0, 0, img.width, img.height, 200, 1178, 100, 100);
	    	resolve();
	    };
	    img.onerror = function() {
	    	throw new Error('图片加载失败!');
	    }
	    img.src= this.userIconUrl+'?ts='+new Date();
```

但是加了`crossOrigin=anonymous`之后，还是偶尔会出现问题，后来网上介绍在img的url后面加个时间戳就能解决问题。



#### 别的方法

这个方法比较麻烦，如果上述方面没问题，不推荐使用.

通过ajax请求把文件转为blob对象，然后通过`URL.createObjectURL`转换成src可用的地址

````
//直接读成blob文件对象
function getImageBlob (url, cb) {
   var xhr = new XMLHttpRequest();
   xhr.open('get', url, true);
   xhr.responseType = 'blob';
   xhr.onload = function () {
     if (this.status == 200) {
       imgResponse = this.response;
       //这里面可以直接通过URL的api将其转换，然后赋值给img.src
       //也可以使用下面的preView方法将其转换成base64之后再赋值
       img.src = URL.createObjectURL(this.response);
     }
    };
    xhr.send();
  }
  //这里面将blob转换成base64
  function preView (url) {
    let reader = new FileReader();
    getImageBlob(url, function (blob) {
       reader.readAsDataURL(blob);
    });
    reader.onload = function (e) {
      console.log(e.loaded)
    }
  }
  img.onload = function () {
    canvas.width = img.width;
    canvas.height = img.height + 200;
    ctx.drawImage(img, 0, 0);
    document.getElementById('canvasImg').src = canvas.toDataURL("image/jpeg", 1);
  }
  var imgResponse = '';
 getImageBlob('http://wx.qlogo.cn/mmopen/vi_32/RnLIHfXibgFHlticiclzflpriaLsC3TS9b2Sbj05Wh3vGlhcFutt18dfkXGUt8x11e4q2KHlX4EHHaBb6XylLQW1kQ/0');
````



