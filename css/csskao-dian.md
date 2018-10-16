# CSS考点

* 页面导入样式时，使用`link`和`@import`有什么区别？

```
1.link属于XHTML标签，除了加载CSS外，还能用于定义RSS，定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS。
2.页面被加载时，link会同时被加载，而@import引用的CSS会等页面被 加载完再加载。
3.import是CSS2.1提出的，只有IE5以上才能被识别，而link是XHTML标签，无兼容问题。
4.link支持使用JS控制DOM去改变样式，而@import不支持。
```

* 介绍一下标准的`CSS`的盒子模型？低版本的IE盒子模型有什么不同？

```
两种模型：IE盒子模型、标准盒子模型
标准盒子模型：宽高为content
IE盒子模型：宽高为content+padding+border
```

* `CSS`选择符有哪些？哪些属性可以继承？哪些属性不可继承？

```
* 1.id选择器（#id）
  2.类选择器（.class）
  3.标签选择器（div）
  4.相邻选择器(div + p)
  5.子选择器（ul > li）
  6.后代选择器（li a）
  7.通配符选择器（*）
  8.属性选择器(a[rel='external'])
  9.伪类选择器（a:hover li:nth-child）

* 可继承的属性：
  1.字体系列属性:
    font-family、font-weight、font-size、font-style等
  2.文本系列属性:
    text-indent、text-align、line-height、word-spacing、letter-spacing、text-transform、direction、color
  3.元素可见性：visibility
  4.表格布局属性：
    caption-side、border-collapse、border-spacing、empty-cells、table-layout
  5.列表布局属性：
    list-style-type、list-style-image、list-style-position、list-style
  6.生成内容属性：quotes
  7.光标属性：cursor
  8.页面样式属性：
    page、page-break-inside、windows、orpgabs
  9.声音样式属性：
    speak、volume、voice-family、pitch、stress、richness、azimuth、elevation

* 不可继承属性：
  1.框类型：display
  2.文本属性：
    vertical-align、text-decoration、text-shadow、white-space、unicode-bidi
  3.盒子模型属性：
    width、height、margin、border、padding
  4.背景属性：background
  5.定位属性：
    float、clear、position、top、right、bottom、left、min-width、overflow、clip、z-index
  6.生成内容属性：
    content、counter-reset、counter-increment
  7.轮廓样式属性：
    outline-style、outline-width、outline-color、outline
  8.页面样式属性：
    size、page-break-before、page-break-after
  9.声音样式属性：
    pause-before、pause-after、pause、cue-before、cue-after、cue、play-during
```

* `CSS`优先级算法如何计算？

```
* 优先级就近原则，同权重情况下样式定义最近者为准；
* 载入样式以最后载入的定位为准。

优先级算法：
    同权重：内联样式表（标签内部） > 嵌入样式表（当前文件中） > 外部样式表（外部文件）
    !important > id > class > tag
    important比内联优先级高
```

* 如何居中`div`？
  * 水平居中：给`div`设置一个宽度，然后添加`margin: 0 auto;`属性
  * 绝对定位的`div`居中

```css
水平垂直居中：
** 设置外边距 **
div {
    position: relative;        /* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    margin: -150px 0 0 -250px;         /* 外边距为自身宽高的一半 */
    background-color: red;         /* 方便看效果 */
}

** 利用transform属性 **
div {
     position: absolute;        /* 相对定位或绝对定位均可 */
     width:500px;
     height:300px;
     top: 50%;
     left: 50%;
     transform: translate(-50%, -50%);
     background-color: red;         /* 方便看效果 */
}

 ** 使用flex布局 **
.container {
    display: flex;
    align-items: center;         /* 垂直居中 */
    justify-content: center;            /* 水平居中 */
}
.container div {
    width: 100px;
    height: 100px;
    background-color: pink;        /* 方便看效果 */
}
```

* `position`的`absolute`与`fixed`共同点与不同点

```
共同点：
1.改变行内元素的呈现方式，display被设置为inline-block
2.让元素脱离普通流，不占据空间
3.默认会覆盖到非定位元素上

不同点：
absolute的“根元素”是可以设置的，而fixed的“根元素”固定为浏览器窗口
```



