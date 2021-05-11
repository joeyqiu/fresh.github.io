## Button组件

button组件点击的时候，会通过css加一个动画效果，具体的动画效果代码在other.less中。

```
styleForPseudo.innerHTML = `
      [${getPrefixCls('')}-click-animating-without-extra-node='true']::after, .${getPrefixCls(
        '',
      )}-click-animating-node {
        --drip-wave-shadow-color: ${waveColor};
      }`;
      if (!document.body.contains(styleForPseudo)) {
        document.body.appendChild(styleForPseudo);
      }
```



```
components/style/core/motion/other.less

@click-animating-with-extra-node-true-after: ~'@{click-animating-with-extra-node-true}::after';

@{click-animating-with-extra-node-true-after},
.@{drip-prefix}-click-animating-node {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: block;
  border-radius: inherit;
  box-shadow: 0 0 0 0 @primary-color;
  box-shadow: 0 0 0 0 var(--drip-wave-shadow-color);
  opacity: 0.2;
  animation: fadeEffect 2s @ease-out-circ, waveEffect 0.4s @ease-out-circ;
  animation-fill-mode: forwards;
  content: '';
  pointer-events: none;
}

@keyframes waveEffect {
  100% {
    box-shadow: 0 0 0 @primary-color;
    box-shadow: 0 0 0 @wave-animation-width var(--drip-wave-shadow-color);
  }
}

@keyframes fadeEffect {
  100% {
    opacity: 0;
  }
}

```

