# translate卡顿

在某些手机上，比如上下轮播的组件，通过`translateY` 来移动的时候，会特别的快速，没有那种动画的流畅感

```
&.leave {
			animation-name: carousel-leave;
		}
		&.enter {
			animation-name: carousel-enter;
		}

@keyframes carousel-enter {
  from{ transform: translateY(0); }
  to{ transform: translateY(-100%); }
}
@keyframes carousel-leave {
  from{ transform: translateY(-100%); }
  to{ transform: translateY(-200%); }
}
```



但是把`translateY` 改成`translate3d` 之后就流畅了，所以以后默认使用translate3d

```
@keyframes carousel-enter {
  from{ transform: translate3d(0,0,0); }
  to{ transform: translate3d(0,-100%,0); }
}
@keyframes carousel-leave {
  from{ transform: translate3d(0,-100%,0); }
  to{ transform: translate3d(0,-200%,0); }
}
```



还能通过`will-change` 属性来默认启动硬件加速。在需要动的时候添加，默认不启动，避免性能消耗。