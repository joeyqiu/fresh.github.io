## 用CSS为英文和中文字体分别设置不同的字体

font-family的调用方法:

````
div {
    font-family:Arial,'Times New Roman','Microsoft YaHei',SimHei;   
    font:bold 12px/0.75em Arial,'Times New Roman','Microsoft YaHei',SimHei;  
}
````

根据font-family的原则,假如客户终端不认识前面的字体,就自动切换到第二种字体,第二种不认识就切换到第三种,以此类推.假如都不能识别就调用默认字体

根据font-family的字体调用原则我们可以为英文,中文,等两种字体调用不同的字体来渲染.

如:`Arial,’Times New Roman’`这两种字体不认识中文,只认识英文,所以,这两种字体只能渲染英文数字和一些特殊符号,而页面中的中文就会自动调用第三种字体Microsoft YaHei(PS:假如存在这种字体的话).

所以,在定义字体的时候把英文的字体写在前面把中文的写在后面.这样，系统就会自动按顺序依次给字用字体，如果当前字体不支持文本，自动换用列表中的下一个字体