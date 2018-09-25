# CSS考点

* 页面导入样式时，使用`link`和`@import`有什么区别？

```
1.link属于XHTML标签，除了加载CSS外，还能用于定义RSS，定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS。
2.页面被加载时，link会同时被加载，而@import引用的CSS会等页面被 加载完再加载。
3.import是CSS2.1提出的，只有IE5以上才能被识别，而link是XHTML标签，无兼容问题。
4.link支持使用JS控制DOM去改变样式，而@import不支持。
```



