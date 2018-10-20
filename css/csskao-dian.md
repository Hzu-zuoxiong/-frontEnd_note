# CSS考点

* 页面导入样式时，使用`link`和`@import`有什么区别？

```
1.link属于XHTML标签，除了加载CSS外，还能用于定义RSS，定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS。
2.页面被加载时，link会同时被加载，而@import引用的CSS会等页面被 加载完再加载。
3.import是CSS2.1提出的，只有IE5以上才能被识别，而link是XHTML标签，无兼容问题。
4.link支持使用JS控制DOM去改变样式，而@import不支持。
```

* CSS3有哪些新特性？

```
1.CSS3实现圆角(border-radius)，阴影(box-shadow)
2.对文字加特效(text-shadow)，线性渐变(gradient)，旋转(transform)
3.transform:rotate(9deg) scale(0.85, 0.90) translate(0px -30px) skew(-9deg, 0deg); // 旋转、缩放、定位、倾斜
4.增加了更多的CSS选择器  多背景 rgba
5.CSS3唯一引入的伪类： ::selection
6.媒体查询，多栏布局
7.border-image
```

* 介绍一下标准的`CSS`的盒子模型？低版本的IE盒子模型有什么不同？

```
两种模型：IE盒子模型、标准盒子模型
标准盒子模型：宽高为content
IE盒子模型：宽高为content+padding+border
```

* `CSS`选择符有哪些？哪些属性可以继承？优先级算法如何计算？CSS3新增伪类有哪些？

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

优先级算法：
1.优先级就近原则，同权重情况下样式定义最近者为准
2.载入样式以最后载入的定位为准
3.!important > id > class > tag
4.important比内联优先级高

CSS3新增伪类举例：
p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。
p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。
:enabled :disabled 控制表单控件的禁用状态。
:checked        单选框或复选框被选中。
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

* 解析下CSS sprites，以及你要如何在页面或网站中使用它？

```
CSS sprites 就是把网页中一些被背景图片整合到一张图片文件中，再利用CSS的background-image、background-repeat、
background-position的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置。这样可以减少很多图片请求
的开销，因为请求消耗时比较长；请求虽然可以并发，但也有限制，一般浏览器都是6个。
```

* CSS预处理器有哪些优缺点？

```
优点：
1.用变量存储多次引用的信息，只需要改动一个地方，就能让所有引用之处随之改变
2.新语法中的混合（mixin）能够重用一段样式代码
3.内置丰富的函数
4.可以像JS那也使用数学运算、条件判断和循环
5.选择器可嵌套选择器，沿着嵌套的选择器链向上组合形成最终的选择器，嵌套的形式模拟出了HTML的层级结构，同时形成了命名空间，选择器
  之间的关系更明显，增强了文件的可读性。
6.导入规则可让各部分代码保持独立，模块化管理

缺点：
1.通过编译生成CSS文件，降低了对CSS文件的控制力，如果书写不当，那么编译出的CSS文件将会巨大而复杂
2.调试难度增加
3.带来一定的学习成本
```

* 当出现外边距塌陷时，外边距之间的计算方式是怎样的？

```
元素的外边距可以用正数或负数指定，使用不同的组合会改变外边距的计算方式：
1.两个都是正数，取较大的值
2.两个都是负数，取绝对值较大的值
3.一正一负，取两个值相加的和
```

* display: none与visibility: hidden都可隐藏元素，二者有何区别？

```
display:none：将CSS属性display定义为none后，相当于元素没有了后代元素，在正常流中不占用任何空间，元素的真实尺寸将会丢失，还会
导致浏览器的重排（reflow）和重绘（repaint）。
visibility:hidden：将CSS属性visibility定义为hidden后，在正常流中还是会占用空间，仍具有元素的真实尺寸，只会导致浏览器重绘。

1).父元素用display隐藏后，子元素中的背景图像不会加载
<div style="display:none;">
    <div style="background:url(lake.png)"></div>
</div>
2).父元素用visibility隐藏后，子元素中的背景图像仍然会加载
<div style="visibility:hidden;">
    <div style="background:url(lake.png)"></div>
</div>
```



