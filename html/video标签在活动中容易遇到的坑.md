## Video标签在活动开发中容易踩坑的点



近期做了几个与视频有关的项目，现在想把一些关于video标签的处理经验给大家分享一下，有没有提到的地方或者新的坑点欢迎大家补充。

**video标签最基本的使用**

*- 标签属性：* 

 src：视频的URL 

 poster：视频封面，没有播放时显示的图片 

 preload：预加载 

 autoplay：自动播放

 loop：循环播放 

 controls：浏览器自带的控制条 

 width：视频宽度 

 height：视频高度 

 videoWidth：视频内容宽度（初始状态下为0，加载metadata之后才有值） 

 videoHeight：视频内容高度（初始状态下为0，加载metadata之后才有值）

*- Media方法和属性(HTMLVideoElement 和HTMLAudioElement 均继承自HTMLMediaElement)：*

**错误状态** 

Media.error; //null:正常

Media.error.code; //1.用户终止 2.网络错误 3.解码错误 4.URL无效 

 **网络状态**

Media.currentSrc; //返回当前资源的URL 

 Media.src = value; //返回或设置当前资源的URL 

 Media.canPlayType(type); //是否能播放某种格式的资源 

 Media.networkState; //0.此元素未初始化 1.正常但没有使用网络 2.正在下载数据 3.没有找到资源 

 Media.load(); //重新加载src指定的资源 

 Media.buffered; //返回已缓冲区域，

TimeRanges Media.preload; //none:不预载 metadata:预载资源信息 auto: 

**准备状态**

 Media.readyState; //1:HAVE_NOTHING 2:HAVE_METADATA 3.HAVE_CURRENT_DATA 4.HAVE_FUTURE_DATA 5.HAVE_ENOUGH_DATA 

 Media.seeking; //是否正在seeking 

**回放状态**

 Media.currentTime = value; //当前播放的位置，赋值可改变位置 

 Media.startTime; //一般为0，如果为流媒体或者不从0开始的资源，则不为0 

 Media.duration; //当前资源长度 流返回无限 

 Media.paused; //是否暂停 

 Media.defaultPlaybackRate = value;//默认的回放速度，可以设置 

 Media.playbackRate = value;//当前播放速度，设置后马上改变 

 Media.played;//返回已经播放的区域，TimeRanges，关于此对象见下文 

 Media.seekable;//返回可以seek的区域TimeRanges 

 Media.ended; //是否结束 

 Media.autoPlay;//是否自动播放 

 Media.loop; //是否循环播放 

 Media.play(); //播放 

 Media.pause(); //暂停 

**控制**

 Media.controls;//是否有默认控制条 

 Media.volume = value; //音量 

 Media.muted = value; //静音 

**TimeRanges(区域)对象**

 TimeRanges.length; //区域段数 

 TimeRanges.start(index) //第index段区域的开始位置 

 TimeRanges.end(index) //第index段区域的结束位置



*- Media相关的事件：* 

 "loadstart"：客户端开始请求数据 

 "progress"：客户端正在请求数据 

 "suspend"：延迟下载 

 "abort"：客户端主动终止下载（不是因为错误引起），

"error"：请求数据时遇到错误 

 "stalled"：网速失速

 "play"：play()和autoplay开始播放时触发 

 "pause"：pause()触发 

 "loadedmetadata"：成功获取资源长度 

 "loadeddata"： 

 "waiting"：等待数据，并非错误 

 "playing"：开始回放

 "canplay"：可以播放，但中途可能因为加载而暂停 

 "canplaythrough"：可以播放，歌曲全部加载完毕 

 "seeking"：寻找中 

 "seeked"：寻找完毕 

 "timeupdate"：播放时间改变 

 "ended"：播放结束 

 "ratechange"：播放速率改变 

 "durationchange"：资源长度改变 

 "volumechange"：音量改变

"stalled"： 视频没有播放，没有取回任何媒介数据：一般是由于网络状况不佳，导致视频下载中断，可能在play()事件触发前

**
**

**video处理常见的忽略的点**

**- 音频混播的处理**

音频混播指的H5页面内的video/audio与原生的音乐播放器/视频播放器处于同时播放的状态，影响用户体验，一般遇到带有视频的活动产品都会要求处理混播，在京东APP上面让H5的音频播放获取音频焦点，暂时原生只对iOS 6.5.3及以上版本，提供了阻止混播模式的处理方案：

[H5通知原生音视频播放状态](http://cf.jd.com/pages/viewpage.action?pageId=98926267)

Android方面暂时没有处理混播的方案，这个需要明确告知测试和产品

**- 视频可见性处理(IOS自己会做处理，不需要额外的处理)**

原生上我们H5页面的音视频播放会受到系统的多种操作而被终止，比如说切换页面，锁屏行为，接听电话，这些行为都会触发video标签的pause方法使得音频停止播放，所以比较好的处理方案就是在音视频播放页面可见性切换的时候去处理音视频播放状态，比如当音视频播放页面不可见的时候主动去调用pause()方法，当切换回音视频播放页面时候主动去调用play()方法。 H5有一个事件叫visibilitychange可以用于监听当前页面是否被挂起。如下：

 

```
`document.addEventListener(``"visibilitychange"``, () => { ``  ``if``(document.hidden) {``    ``// 页面被挂起``  ``}``  ``else` `{``    ``// 页面呼出``  ``}``});`
```



但是呢，手Q不会触发这个事件！因为手Q的网页永远都不会被挂起，所以visibilitychange事件不会被触发。但是手Q有一个qbrowserVisibilityChange事件，而且手Q把这个事件绑定到了document上，他的使用方式如下：

```
`document.addEventListener(``"qbrowserVisibilityChange"``, function(e){``  ``if``(e.hidden) {``    ``// 手Q被挂起``  ``}``  ``else` `{``    ``// 手Q被呼起``  ``}``});`
```



微信是支持visibilitychange事件的，但是要知道我们活动项目一般都会有要求在原生+微信+手Q+M站的投放需求，所以一般情况下我们要用这事件可以这么做：

```
`document.addEventListener(``"qbrowserVisibilityChange"``, function(e){``  ``var evt = document.createEvent(``"HTMLEvents"``); ``  ``evt.initEvent(``"visibilitychange"``, ``false``, ``false``); ``  ``evt.hidden = e.hidden; ``  ``document.dispatchEvent(evt); ``}, ``true``); ``document.addEventListener(``"visibilitychange"``, function(e) {``  ``e.hidden = e.hidden === undefined ? document.hidden : e.hidden; ``}, ``true``);`
```

我们把手Q上的qbrowserVisibilityChange以visibilitychange的事件名称对外分发，那么我们实际使用的时候只需要监听visibilitychange事件即可，这里我的做法是把上面的代码封装在一个工具类的js里面，实际在业务里面使用的情况如下：

 

```
`// 初始化WQ挂起事件``   ``initWQVisibleEvent()``   ``document.addEventListener(``'visibilitychange'``, (e) => {``    ``const` `video = ``this``.$refs.video``    ``if` `(e.hidden) {``     ``// 页面被挂起``     ``if` `(!video.paused) {``      ``video.pause()``     ``}``    ``} ``else` `{``     ``// 页面呼出``     ``if` `(video.currentTime > ``0` `&& video.paused) {``      ``video.play()``     ``}``    ``}``   ``})`
```

 

其中具体逻辑可以自己根据业务进行优化更改

**- 视频尺寸的处理**

这里主要提到的是全屏video的处理，正常情况下视频的最佳比例是16 : 9，但是由于现在市面上并非每一款手机都是正常比例，比如IphoneX和三星就是属于瘦长类型，还有的诸如华为和Htc的某些机型属于矮胖类型，所以一味的使用Object-fit: fill/Object-fit: contain/Object-fit: cover并不能给用户带来较好的体验，这里我提供一种动态计算video最佳长宽的方法，我们在dom结构渲染完成之后去拿到当前页面的宽高比例(ratio)，通过比较该比例和16:9的大小去决定该视频是以高为主还是宽为主，比如IphoneX这种机型明显是以高为主的，然后宽度在高的基础上向两边延伸，可以想象如果video的高和屏幕等高，那么这个video的宽度肯定是大于屏幕宽度，这时候就需要结合js和css去动态设置video的left，这里提供我的实现方式：



```
`mounted() {``   ``this``.$nextTick(() => {``    ``// 动态处理video标签宽高适配``    ``const` `video = ``this``.$refs.video``    ``const` `ratio = Math.fround(document.documentElement.clientHeight / document.documentElement.clientWidth)``    ``if` `(ratio >= ``1.78``) {``     ``// 竖屏``     ``video.height = document.documentElement.clientHeight``     ``video.width = Math.floor(video.clientHeight * ``0.572``)``     ``const` `winW = document.documentElement.clientWidth``     ``video.style.left = (winW - video.width) / ``2` `+ ``'px'``    ``} ``else` `{``     ``// 宽屏``     ``video.width = document.documentElement.clientWidth``     ``video.height = Math.floor(video.width * ``1.78``)``     ``const` `winH = document.documentElement.clientHeight``     ``video.style.top = (winH - video.height) / ``2` `+ ``'px'``    ``}``   ``})``  ``}`
```

**video全屏和嵌套播放的处理**

在IOS中video的播放是默认弹出一个全屏的播放器来播放的，如果我们想要做到类似直播APP在video上面盖一些交互按钮例如点赞或者评论，我们可以在video标签里面加上playsinline="true"和 webkit-playsinline="true"属性，在Android里面因为video标签始终在最上层，所以Android上要达到这个效果需要使用基于x5内核的同层播放器，添加这几个属性x5-video-player-type="h5" 和x5-video-player-fullscreen="true"，实际项目效果可以参考：

[京晚八点二期](https://h5.m.jd.com/dev/4FDRK1M6xvafD7wnkBUc3CdpVZoR/index.html#/home)

如果单纯想要嵌套播放的话只需要加上playsinline="true"和 webkit-playsinline="true"这两个属性，宽高自己设置即可

**其他关于controls相关的控制**

**
**

\- 隐藏controls中的音量控制

```
`video::-webkit-media-controls-volume-slider, video::-webkit-media-controls-mute-button { ``//隐藏android端video标签自带的音量调节按钮`` ``display: none;`
```

 

\- 隐藏android端点击control中暂停按钮时视频中心出现的play icon

 

```
`video::-webkit-media-controls-volume-slider, video::-webkit-media-controls-mute-button { ``//隐藏android端video标签自带的音量调节按钮`` ``display: none;``}`
```

 

 

其他关于video的控制黑科技：

 

 video::-webkit-media-controls-panel 

 

 video::-webkit-media-controls-play-button 

 

 video::-webkit-media-controls-volume-slider-container 

 

 video::-webkit-media-controls-volume-slider 

 

 video::-webkit-media-controls-mute-button 

 

 video::-webkit-media-controls-timeline 

 

 video::-webkit-media-controls-current-time-display 

 

 video::-webkit-full-page-media::-webkit-media-controls-panel 

 

 video::-webkit-media-controls-timeline-container 

 

 video::-webkit-media-controls-time-remaining-display 

 

 video::-webkit-media-controls-seek-back-button 

 

 video::-webkit-media-controls-seek-forward-button 

 

 video::-webkit-media-controls-fullscreen-button 

 

 video::-webkit-media-controls-rewind-button 

 

 video::-webkit-media-controls-return-to-realtvideo::ime-button 

 

 video::-webkit-media-controls-toggle-closed-captions-button

 

如果想操作Video标签全屏或者退出全屏的回调事件，或者x5同层播放器的其他属性，参考下面的H5同层播放器的接入规范

[H5同层播放器接入规范](https://x5.tencent.com/tbs/guide/video.html)

 

补充：

video标签的播放，在Android和IOS上的差异：

IOS上如果不做处理是处于全屏播放模式，调用的是IOS自带的播放器，会专门打开一个播放的页面
但是IOS可以在video标签上覆盖元素，前提是设置playsinline="true"属性，让其处于inline播放模式，然后通过js设置宽高使其看起来像是全屏播放

Android上video的缺陷是全屏播放模式下无法在video标签上覆盖元素，除非使用x5浏览器内核自带的同层播放模式，但是使用x5同层播放器会产生新的问题，在x5 同层全屏播放模式下，页面左上角的"返回"按钮其实默认是退出全屏，漏出原生的TitleBar，这个x5有提供相关方法进行操作，并且全屏模式下右上角的分享无法做定制，是x5播放器自带的分享，分享内容为当前页面title
+url，但是我们需要的是正常的分享，这个暂时无法处理

友商的做法(淘宝)：淘宝做这种页面是在一个类似xview的环境，所以他们可以统一使用Inline播放模式，不需要用到x5内核，上面也没有titleBar是一个纯界面，可以自定制返回按钮等

关于静音的处理：

muted属性在ios 10以下不支持，建议做法是在该版本以下去除自定义的静音控制按钮，开启默认行为

**重要更新**

由于深圳侧WQ团队同事给h5.m.jd.com域名添加了白名单，所以该域名下的页面想在video标签上覆盖东西的话可以不用x5的同层播放，白名单的效果和配置x5-playinline属性效果不太一样，配置了白名单可以Inline播放，同时可以去除控制条和右上角的小窗按钮（会出现全屏字样按钮），但是如果非白名单的域名，想要在微信上Inline播放要使用x5-playinline属性，但是使用者属性虽然可以达到Inline效果，点击视频时候会出现一个通用样式黑底的control bar，无法去除，而且不能在视频上面定制化自己的元素。

 

经过调研，市面上可以看到的安卓微信端关于video使用的几种场景：

1.最基本的场景，就是一个video标签，不需要任何别的属性，默认点击播放后会进入x5内置的统一播放器，直接全屏播放被微信接管，不能在视频上面添加元素；

2.需要inline播放的视频，这种只需要加x5-playinline属性，就不会直接默认全屏播放了，但是会出现微信自带的control bar和小窗按钮（没办法去掉），也不能在视频上面添加元素；

3.x5同层播放，需要加上x5-video-player-type="h5" 属性，就可以进入一种放大之后的全屏播放效果，但是会出现同层播放器的返回按钮和右上角折叠按钮（没办法去掉），control bar可以去掉，可以在视频上面添加元素，缺点是返回上级页面会先进入没放大之前的状态，分享也会有问题；

4.添加了白名单的video标签，加上playsinline和webkit-playsinline属性就可以实现内联播放，可以去掉control bar，也可以在视频上面添加元素，缺点就是右上角会出现全屏字样的按钮（没办法去掉，用户点击之后就会进入x5全屏播放模式，会脱离我们的控制，产品不能接受）；

5.腾讯自身的视频资源，会出现别的特点，但是于我们而言基本用不上。

**所有最后采取的方案是第四种，采用放大视频居中显示的样式，将全屏按钮顶到屏幕外，类似cover显示效果。**



[流媒体播放基本了解](https://www.villainhr.com/page/2017/01/19/流媒体播放基本了解)

