## url变化后页面不更新

Q:

通过`this.props.history.push` 或者 `this.props.history.replace`等方法修改url之后，发现页面没有刷新，还保持着之前的旧数据和状态，why?



A:

其实页面中的数据流是会刷新的，通过props来更新。

上述问题是页面中带有了state数据，如果 state数据在`componentDidMount`的时候初始化了，更新url的时候不会重复触发didMount事件，所以state会保留着旧数据，看着就像是没刷新一样。



可以在 `componentDidUpdate`中刷新下state的数据。

