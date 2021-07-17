## span元素底部对齐

如果一行的几个span元素，想要底部对齐的展示，然后对父元素没有什么要求的话，默认好像就是对齐的。

```
<div class="coupon_item_quan_info">
	<span class="text_yang ">¥</span>
	<span class="text_amount ">70</span>
	<span class="text_title ">满90元可用</span>
</div>
```

如果没有特殊处理，其实默认都是底部对齐的，，，这边因为title元素需要设定为inline-block，所以设置vertical-align: sub (垂直对齐文本的下标。) 

```
.coupon_item_quan_info {}
  
  .text_yang{
    font-family: JDZhengHei-01-Regular;
    font-size: 24px;
    font-weight: bold;
    color: #FF2000;
  }
  
  .text_amount{
    font-family: JDZhengHei-01-Regular;
    font-size: 36px;
    font-weight: bold;
    color: #FF2000;
  }

  
  .text_title{
    font-family: PingFangSC-Regular; 
    font-size: 20px;

    max-width: 120px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-left: 20px;
    display: inline-block;
    vertical-align: sub;
  }
```



如果对父元素的高度等有要求，可能只能通过margin或者line-height来控制展示了。目前没看到什么通用的方法。