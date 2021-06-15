## CreateJS

整个CreateJS引擎由如下几个部分分别组成：

* EASELJS
* TWEENJS
* SOUNDJS
* PRELOADJS
* ZOE



### EASELJS

提供canvas绘画的功能，使得2D绘画更加的简单。



### TWEENJS

provides a simpel but powerful tweeting interface. It supports tweeting of both numeric object properties & CSS style properties, and allow you to chain tweens and actions together to create omplex sequences



### SOUNDJS

manages the playback of audio on the web. It works via plugins which abstract the actual audio implementation, so playback is possible on any platform without specific knowledge of what mechanisms are necessary to play sounds.



### PRELOADJS

provides a consistent way to preload content for use in HTML applications. Preloading can be done using HTML tags, as well as XHR.

默认情况下，会优先使用XHR来预加载，因为支持加载进度条和完成事件。如果遇到一些不兼容的情况，则会使用标签加载来取代。



### ZOE

用来创建雪碧图相关的

SWF animations in Adobe Flash/Animation can be exported to SpriteSheets using Zoe.

从Adobe的动画来转成EaselJS支持的雪碧图动画，其实有几种：

* Exporting SpriteSheets or HTML5 content from Adobe Flash/Animation supports the EaselJS SpriteSheet format.
* The popular __Texture Packer__ has EaselJS support
* zoe



