## !important in keyframe

在某些浏览器中，keyframes中定义的`!important` 属性是被忽略的，所以不要使用。



```css
@keyframes important1 {
  from { margin-top: 50px; }
  50%  { margin-top: 150px !important; } /* ignored */
  to   { margin-top: 100px; }
}

@keyframes important2 {
  from { margin-top: 50px;
         margin-bottom: 100px; }
  to   { margin-top: 150px !important; /* ignored */
         margin-bottom: 50px; }
}
```



