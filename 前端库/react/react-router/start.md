## ReactRouter

If you are writing an application that will run in the browser, you should instead install `react-router-dom`.



从安装以来来看，需要看`react-router`和`react-router-dom`两个库的源代码



思路：

* HashRouter: 主要通过修改hash值来添加浏览记录，并进行页面切换
* BrowserRouter: 通过H5的history对象的pushState, replaceState方法（如果不支持，则直接通过location.href来进行降级）



````
<Router>
	<Switch>
		<Route></Route>
	</Switch>
</Router>


````



前端一般都是通过Swtich来包裹的，选取Route人中第一个匹配的，只渲染一个。

每次hash或者history修改后，Switch这边会需要重新匹配下，然后重新渲染。