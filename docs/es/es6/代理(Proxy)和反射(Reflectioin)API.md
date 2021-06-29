代理Proxy是一种可以拦截并改变底层JavaScript引起操作的包装器，在新语言中通过它暴露内部运作的对象。



### 代理和反射

通过`new Proxy()`可创建代替其他目标（target）对象的代理，它虚拟化了目标，所以两者看起来功能一致。

代理可以拦截Javascript引擎内部目标的底层对象操作，这些底层操作被拦截后会触发响应特定操作的陷阱函数。

反射API以`Reflect对象`的形式出现，对象中方法的默认特性与相同的底层操作一致，而代理可以覆写这些操作，每个代理陷阱对应一个命名和参数都相同的Reflection方法。

| 代理陷阱                 | 覆写的特性                                                   | 默认特性                           |
| ------------------------ | ------------------------------------------------------------ | ---------------------------------- |
| get                      | 读取一个属性值                                               | Reflect.get()                      |
| set                      | 写入一个属性                                                 | Reflect.set()                      |
| has                      | in操作符                                                     | Reflect.has()                      |
| deleteProperty           | delete操作符                                                 | Reflect.deleteProperty()           |
| getPrototypeOf           | Object.getPrototypeOf()                                      | Reflect.getPrototypeOf()           |
| setPrototypeOf           | Object.setPrototypeOf()                                      | Reflect.setProtorypeOf()           |
| isExtensible             | Object.isExtensible()                                        | Reflect.isExtensible()             |
| preventExtensions        | Object.preventExtensions()                                   | Reflect.preventExtensions()        |
| getOwnPropertyDescriptor | Object.getOwnPropertyDescriptor()                            | Reflect.getOwnPropertyDescriptor() |
| defineProperty           | Object.defineProperty()                                      | Reflect.defineProperty()           |
| ownKeys                  | Object.keys()、Object.getOwnPropertyNames()、Object.getOwnPropertySymbols() | Reflect.ownKeys()                  |
| apply                    | 调用一个函数                                                 | Reflect.apply()                    |
| construct                | 用new调用一个函数                                            | Reflect.construct()                |

