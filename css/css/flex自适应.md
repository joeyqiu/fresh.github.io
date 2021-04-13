## flex自适应

当一行三列元素的时候，使用flex自适应是一种很好的方式。

左右两边固定的宽度，中间的宽度自适应，而且文本超出的时候缩略显示



```
<div className="bottom-area">
    <i className="bottom-icon bg" />
    <div className="bottom-info">
    <p className="title">开通PLUS会员</p>
    <p className="tip ellipse">更多专属超级特权等着您更多专属超级特权等着您更多专属超级特权等着您</p>
    </div>
    <div className="bottom-btn">
    <div className="inner">立即开通</div>
    </div>
</div>
```



```
.bottom-area {
	position: fixed;
	width: 100%;
	height: 105px;
	left: 0;
	bottom: 0;
	background-color: #fff;
	display: flex;
	align-items: center;
	justify-content: center;
	box-shadow: 0 3px 18px 2px rgba(164,164,164,0.23);

	.bottom-icon {
		flex-shrink: 0;
		display: inline-block;
		margin-left: 50px;
		margin-right: 30px;
		width: 71px;
		height: 73px;
		background-image: url('https://img30.360buyimg.com/pop/jfs/t24310/74/2290419899/1721/32847c44/5b7bbf43N5cec4fbe.png');
	}

	.bottom-info {
		flex: 1;
		overflow: hidden;
		font-size: 30px;

		> .title {
			color: #001A44;
		}

		> .tip {
			font-family: PingFangSC-Light;
			font-size: 24px;
			color: #666;
		}
	}

	.bottom-btn {
		flex-shrink: 0;
		margin-right: 29px;
		color: #001A44;
		font-size: 26px;
		> .inner {
			width: 183px;
			height: 51px;
			border: 2px solid #001A44;
			border-radius: 27px;
			display: flex;
			align-items: center;
			justify-content: center;
		}
	}
}
```



上述的`.bottom-info`文本宽度超出，如果不添加`overflow: hidden;`的时候，自适应失效。