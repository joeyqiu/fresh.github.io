## video标签在ios上禁止全屏

 

目前video标签，在ios手机上点击的时候是会自动进入全屏模式的，android上则不会。ios上可以通过添加如下属性来禁止。

```html
<video className="video-item"
       ref={this.video}
       src={videoUrl}
       preload="meta"
       width="100%"
       height="100%"
       controls="controls"
       webkit-playsinline="true"
       x5-playsinline="true"
       playsinline="true"/>
```

`webkit-playsinline`和`playsinline`属性。

`x5-playsinline=""`，用于微信浏览器。



## 遇到的video问题

### ios上播放

播放的时候，切换到后台，会自动暂停，并且点击后播放，会直接从头开始播放。

解决：通过visibilitychange的监听，来获取切换隐藏时的播放进度

```javascript
addVisibilityListener() {
		document.addEventListener('visibilitychange',()=>{
      try {
      	const videoItem = this.video.current;
      	if(document.visibilityState == 'hidden'){
	        videoItem.pause()
	        this.currentTime = videoItem.currentTime;
	      } else {
	      	videoItem.currentTime = this.currentTime;
	      }
      } catch(e){console.log(e)}
    });
	}
```





### android上播放

在视频的全屏播放切换时，从全屏返回，视频会自动暂停，而且切换的时候会重新播放。

目前想着是不是因为视频源的问题，在android手机上，会从关键帧开始播放，然后视频比较短的时候，总是从头开始播放，换一个长度比较长的视频源，就会好很多。

但是android上也有切换后台的问题，在上述代码的情况下，再加上全屏切换的检测，，，

```javascript
// android下才添加监听全屏切换问题
	addFullScreenListener() {
		const that = this;
		window.onresize = function() {
			try {
				const videoItem = that.video.current;
				that.currentTime = videoItem.currentTime;
	    	videoItem.pause();
	    	setTimeout(()=>{
	    		videoItem.currentTime = that.currentTime+'';
	    		setTimeout(()=>{videoItem.play();}, 100);
	    	}, 100);
			} catch(e){console.log(e)}
		}
	}
```

检测到全屏切换的时候进行记录播放时间，但是短视频也是没有效果。





手机端快进后回到进度跳到开始位置
发现在微信上如果对视频做出快进操作后，视频会莫名对从头播放（完全随机）。

经过对与视频多媒体对学习和查看资料得出结论：

关键帧：
视频中对关键帧是有限的，理论上我们快进的话只可以跳转到关键帧，因为非关键帧是无法播放的。

播放器对于视频快进的处理方式：
高性能解决方式：
当我们在电脑端浏览器对视频快进的时候，如果目标时间点不是关键帧，浏览器会对该部分视频重新解码，将这一秒变成关键帧，再次编码。所以电脑端或者部分性能强悍的播放器是可以随便跳转的。

低性能解决方式：
播放器会自动跳转到该时间点最近到关键帧。但是如果该时间点与关键帧的播放时间过长，将会重头播放视频。（目前发现手机端的微信H5播放器就是这样的）



