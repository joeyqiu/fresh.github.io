# Introduction

zepto移动端源码解析



| module     | default | description                                                  |
| ---------- | ------- | ------------------------------------------------------------ |
| zepto      | ✔       | Core module; contains most methods                           |
| event      | ✔       | Event handling via `on()` & `off()`                          |
| ajax       | ✔       | XMLHttpRequest and JSONP functionality                       |
| form       | ✔       | Serialize & submit web forms                                 |
| ie         | ✔       | Add support for Internet Explorer 10+ on desktop and Windows Phone 8. |
| Detect     |         | Provides `$.os` and `$.browser` information                  |
| Fx         |         | The `animate()` method                                       |
| fx_methods |         | Animated `show`, `hide`, `toggle`, and `fade*()` methods.    |
| Assets     |         | Experimental support for cleaning up iOS memory after removing image elements from the DOM. |
| data       |         | A full-blown `data()` method, capable of storing arbitrary objects in memory. |
| deferred   |         | Provides `$.Deferred` promises API. Depends on the "callbacks" module.  When included, `$.ajax()` supports a promise interface for chaining callbacks. |
| callbacks  |         | Provides `$.Callbacks` for use in "deferred" module.         |
| selector   |         | Experimental `jQuery CSS extensions` support for functionality such as `$('div:first')` and `el.is(':visible')`. |
| touch      |         | Fires tap– and swipe–related events on touch devices. This works with both `touch` (iOS, Android) and `pointer` events (Windows Phone). |
| gesture    |         | Fires pinch gesture events on touch devices                  |
| stack      |         | Provides `andSelf` & `end()` chaining methods                |
| ios3       |         | String.prototype.trim and Array.prototype.reduce methods (if they are missing) for compatibility with iOS 3.x. |



* [core](./core/core.md){:target="_blank"}
* [core_$.camelCase](./core/core_camelCase.md){:target="_blank"}
* [core_$.contains](./core/core_contains.md){:target="_blank"}
* [core_$.each](./core/core_each.md){:target="_blank"}
* [core_$.extend](./core/core_extend.md){:target="_blank"}
* [core_$()](./core/core_().md){:target="_blank"}
* [core_$.fn](./core/core_fn.md){:target="_blank"}

