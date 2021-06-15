## runtime

Runtime，以及伴随的manifest数据，主要是指：在浏览器运行过程中，webpack用来连接模块化应用程序所需的所有代码。

包含模块交互时，连接模块所需的加载和解析逻辑，包括已经加载到浏览器中的连接模块逻辑，以及尚未加载模块的延迟加载逻辑。





打包出来的js代码，都是类似：

```
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[1],{xxx},[[26,4,5]]]);
```

或者

```
(window["webpackJsonp"] = window["webpackJsonp"] || []).push([[5],[]]);
```

这样的代码。



上面push的数组中第二个参数是一个对象，且含有第三个数组，，就指明了依赖和起始入口。



### runtime部分

现在看下打包生成的runtime代码

```
var jsonpArray = (window["webpackJsonp"] = window["webpackJsonp"] || []);
// 先给window["webpackJsonp"]赋值，然后也赋值给jsonpArray


var oldJsonpFunction = jsonpArray.push.bind(jsonpArray);
// 把array的push方法赋值给oldJsonpFunction


jsonpArray.push = webpackJsonpCallback;
// window["webpackJsonp"]的push方法用webpackJsonpCallback取代

jsonpArray = jsonpArray.slice(); // 转成数组
for (var i = 0; i < jsonpArray.length; i++) { webpackJsonpCallback(jsonpArray[i]); }
// 遍历执行，但是一般都是空数组哇，为什么还要这步呢？


var parentJsonpFunction = oldJsonpFunction;
// push方法再赋值给parentJsonpFunction

checkDeferredModules();
// 从别的chunks中运行延迟的模块
```



这边打包出来的每个js文件，都算做一个单独的chunk，然后每个chunk都会运行webbpackJsonpCallback，把chunk中包含的模块代码传进来。





这边用来声明已经加载的chunk，这边的chunkId为4，其实就是生成的runtime那部分的chunk。

```
// The module cache
var installedModules = {};

// object to store loaded CSS chunks
var installedCssChunks = { 4: 0};

// object to store loaded and loading chunks
// undefined = chunk not loaded, null = chunk preloaded/prefetched
// Promise = chunk loading, 0 = chunk loaded
var installedChunks = { 4: 0 };
```





### webpackJsonpCallback函数

install a JSONP callback for chunk loading

chunk加载的回调

```
function webpackJsonpCallback(data) {
					// 第一个参数传入的是chunkId的数组，用来标识该chunk
					var chunkIds = data[0];
					// 第二个参数传入的是该chunk中包含的modules，每个modules都有一个对应的Id
					var moreModules = data[1];
					// 第三个参数传入的是入口module的Id，
					var executeModules = data[2];

					// add "moreModules" to the modules object,
					// then flag all "chunkIds" as loaded and fire callback
					var moduleId,
						chunkId,
						i = 0,
						resolves = [];
					for (; i < chunkIds.length; i++) {
						chunkId = chunkIds[i];
						if (installedChunks[chunkId]) {
							resolves.push(installedChunks[chunkId][0]);
						}
						// 设为0，表示该chunk已加载
						installedChunks[chunkId] = 0;
					}
					for (moduleId in moreModules) {
					// 解析传入的模块对象或者数组，并把模块赋值给modules数组
						if (Object.prototype.hasOwnProperty.call(moreModules, moduleId)) {
							modules[moduleId] = moreModules[moduleId];
						}
					}
					if (parentJsonpFunction) parentJsonpFunction(data);

					while (resolves.length) {
						resolves.shift()();
					}
	
					入口模块
					// add entry modules from loaded chunk to deferred list
					deferredModules.push.apply(deferredModules, executeModules || []);

					// run deferred modules when all chunks ready
					return checkDeferredModules();
				}
```



### checkDeferredModules函数

这边的deferredModules就是传递的第三个参数，所有js中，只有入口js有第三个参数，比如这边的：`[[26,4,5]]`

```
function checkDeferredModules() {
					var result;
					for (var i = 0; i < deferredModules.length; i++) {
						var deferredModule = deferredModules[i];
						// [26,4,5]
						var fulfilled = true;
						for (var j = 1; j < deferredModule.length; j++) {
						// 这边的j从1开始，所以就是从4开始，[26,4,5], 26是入口，后面的模块是依赖的chunkId
							var depId = deferredModule[j];
							
							// 4就是最初的runtime模块代码，肯定是已加载的
							// 5是html中通过script 标签直接引入的，引入后运行下webpackJsonpCallback，也是已加载的
							if (installedChunks[depId] !== 0) fulfilled = false;
						}
						
						// 所以这边的fulfilled是满足的
						if (fulfilled) {
							deferredModules.splice(i--, 1);
							// 清理deferredModules数组
							// deferredModule[0]的值是26，这边赋值到__webpack_require__.s,并通过__webpack_require__方法来调用该模块
							result = __webpack_require__(
								(__webpack_require__.s = deferredModule[0])
							);
						}
					}
					return result;
				}
```



### __webpack_require__函数

传入模块Id去加载

```
function __webpack_require__(moduleId) {
					// Check if module is in cache
					if (installedModules[moduleId]) {
						return installedModules[moduleId].exports;
					}
					// Create a new module (and put it into the cache)
					// 创建一个新的模块，并赋值到installedModuels中
					var module = (installedModules[moduleId] = {
						i: moduleId,
						l: false,
						exports: {}
					});

					// Execute the module function
					// 这边的modules[moduleId]是一个函数，所以可以通过call方法来调用
					modules[moduleId].call(
						module.exports,
						module,
						module.exports,
						__webpack_require__
					);

					// Flag the module as loaded
					// 把该module的状态标识为已加载
					module.l = true;

					// Return the exports of the module
					return module.exports;
				}
```



通过引用moduleId: 26的模块，来依次执行。

```
(function(module, exports, __webpack_require__) {
module.exports = __webpack_require__(39);
/***/ })
```



给exports对象上添加`__esModule` 属性

```
__webpack_require__.r = function(exports) {
					if (typeof Symbol !== "undefined" && Symbol.toStringTag) {
						Object.defineProperty(exports, Symbol.toStringTag, {
							value: "Module"
						});
					}
					Object.defineProperty(exports, "__esModule", { value: true });
				};
```



`__webpack_require__.d` 给getter函数定一个a属性，并返回当前getter

`__webpack_require__.n` 则定义了n函数，并通过判断是返回default还是直接的模块 

```
// getDefaultExport function for compatibility with non-harmony modules
				__webpack_require__.n = function(module) {
					var getter =
						module && module.__esModule
							? function getDefault() {
									return module["default"];
							  }
							: function getModuleExports() {
									return module;
							  };
					__webpack_require__.d(getter, "a", getter);
					return getter;
				};
```



## 尚未加载模块的延迟加载逻辑

这边设置了两个动态载入的页面，page2和page3

```
var page2=asyncComponent(function(){
	return Promise.all(/* import() | page2 */
		[__webpack_require__.e(0), __webpack_require__.e(2)])
								.then(__webpack_require__.bind(null, 43));});
					
var page3=asyncComponent(function(){
return Promise.all(/* import() | page3 */[__webpack_require__.e(0), __webpack_require__.e(3)])
							.then(__webpack_require__.bind(null, 44));});
```



这边的`__webpack_require__.e` 代码如下，requireEnsure?所以以前动态载入可以直接用requireEnsure函数？

```
__webpack_require__.e = function requireEnsure(chunkId) {
					var promises = [];

					// mini-css-extract-plugin CSS loading
					var cssChunks = { "2": 1, "3": 1 };
					// 这边的css模块只有2和3，，，只有传递这两个chunkId的时候，才需要加载
					if (installedCssChunks[chunkId])
						promises.push(installedCssChunks[chunkId]);
					else if (installedCssChunks[chunkId] !== 0 && cssChunks[chunkId]) {
					//如果模块还没有加载，且需要加载，就通过link标签动态载入
						promises.push(
							(installedCssChunks[chunkId] = new Promise(function(
								resolve,
								reject
							) {
								var href =
									"static/css/" +
									({ "2": "page2", "3": "page3" }[chunkId] || chunkId) +
									"." +
									{ "0": "31d6cfe0", "2": "a3057174", "3": "7ff4743b" }[
										chunkId
									] +
									".chunk.css";
								var fullhref = __webpack_require__.p + href;
								var existingLinkTags = document.getElementsByTagName("link");
								for (var i = 0; i < existingLinkTags.length; i++) {
									var tag = existingLinkTags[i];
									var dataHref =
										tag.getAttribute("data-href") || tag.getAttribute("href");
									if (
										tag.rel === "stylesheet" &&
										(dataHref === href || dataHref === fullhref)
									)
										return resolve();
								}
								var existingStyleTags = document.getElementsByTagName("style");
								for (var i = 0; i < existingStyleTags.length; i++) {
									var tag = existingStyleTags[i];
									var dataHref = tag.getAttribute("data-href");
									if (dataHref === href || dataHref === fullhref)
										return resolve();
								}
								var linkTag = document.createElement("link");
								linkTag.rel = "stylesheet";
								linkTag.type = "text/css";
								linkTag.onload = resolve;
								linkTag.onerror = function(event) {
									var request =
										(event && event.target && event.target.src) || fullhref;
									var err = new Error(
										"Loading CSS chunk " +
											chunkId +
											" failed.\n(" +
											request +
											")"
									);
									err.request = request;
									delete installedCssChunks[chunkId];
									linkTag.parentNode.removeChild(linkTag);
									reject(err);
								};
								linkTag.href = fullhref;

								var head = document.getElementsByTagName("head")[0];
								head.appendChild(linkTag);
							}).then(function() {
								installedCssChunks[chunkId] = 0;
							}))
						);
					}

					// JSONP chunk loading for javascript

					var installedChunkData = installedChunks[chunkId];
					if (installedChunkData !== 0) {
						// 0 means "already installed".

						// a Promise means "currently loading".
						if (installedChunkData) {
							promises.push(installedChunkData[2]);
						} else {
							// setup Promise in chunk cache
							var promise = new Promise(function(resolve, reject) {
								installedChunkData = installedChunks[chunkId] = [
									resolve,
									reject
								];
							});
							promises.push((installedChunkData[2] = promise));
	
							// 如果该js chunk也没有加载，也是通过script标签动态载入
							// start chunk loading
							var script = document.createElement("script");
							var onScriptComplete;

							script.charset = "utf-8";
							script.timeout = 120;
							if (__webpack_require__.nc) {
								script.setAttribute("nonce", __webpack_require__.nc);
							}
							script.src = jsonpScriptSrc(chunkId);

							onScriptComplete = function(event) {
								// avoid mem leaks in IE.
								script.onerror = script.onload = null;
								clearTimeout(timeout);
								var chunk = installedChunks[chunkId];
								if (chunk !== 0) {
									if (chunk) {
										var errorType =
											event && (event.type === "load" ? "missing" : event.type);
										var realSrc = event && event.target && event.target.src;
										var error = new Error(
											"Loading chunk " +
												chunkId +
												" failed.\n(" +
												errorType +
												": " +
												realSrc +
												")"
										);
										error.type = errorType;
										error.request = realSrc;
										chunk[1](error);
									}
									installedChunks[chunkId] = undefined;
								}
							};
							var timeout = setTimeout(function() {
								onScriptComplete({ type: "timeout", target: script });
							}, 120000);
							script.onerror = script.onload = onScriptComplete;
							document.head.appendChild(script);
						}
					}
					return Promise.all(promises);
				};
```



所以这边简要下来，就是先通过`__webpack_require__.e`方法把需要的js和css文件动态载入，然后执行`__webpack_require__.bind(null, 43)` 中moduleId为43 的模块

```
asyncComponent(function(){
	return Promise.all([__webpack_require__.e(0), __webpack_require__.e(2)])
								.then(__webpack_require__.bind(null, 43));});
```

