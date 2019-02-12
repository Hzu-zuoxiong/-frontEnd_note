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

* 解析下`CSS sprites`，以及你要如何在页面或网站中使用它？

```
CSS sprites 就是把网页中一些被背景图片整合到一张图片文件中，再利用CSS的background-image、background-repeat、
background-position的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置。这样可以减少很多图片请求
的开销，因为请求消耗时比较长；请求虽然可以并发，但也有限制，一般浏览器都是6个。
```

* `CSS`预处理器有哪些优缺点？

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

* `display: none`与`visibility: hidden`都可隐藏元素，二者有何区别？

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

* `CSS`中类选择器和`ID`选择器有哪些区别？

```
1.类选择器是以点号(.)开头，ID选择器是以井号(#)开头
2.类选择器根据class属性的值选择元素，ID选择器根据id属性的值选择元素
3.类选择器可以应用于多个元素，ID选择器只能应用于一个元素
4.ID选择器的权重比类选择器的要高
```

* `a`链接有四种状态包括未访问\(`:link`\)、已访问\(`:visited`\)、激活\(`.active`\)和悬停\(`:hover`\)，声明的顺序是怎样的。

```
推荐使用LVHA(love hate)的顺序，这四种状态的特殊性相同，因此影响权重的只有在样式表中所处的位置了。
1.当鼠标悬浮在未访问或已访问链接时，会存在两种状态:link与:hover或:visited与:hover，如果:hover声明在:link或:visited之前，
  那么:hover就会覆盖掉。
2.当鼠标点中链接时，会存在两种状态:active和:hover，如果:active声明在:hover之前，那么会被覆盖掉。
3.因此:hover与:active必须在:link与:visited之后，而:active必须在:hover之后，至于:link与:visited它们两的顺序可以互换。
```

* 用过`calc()`函数吗？它是什么，有什么作用？

```
calc()是CSS的一个函数，只有一个数学表达式参数，此函数可处理加减乘除等数学运算，并且在表达式中可混用不同的单位，代码如下。
div {
    width: calc(50%-2px)
}
在用百分比做自适应布局的时候，如果进行计算会比较困难，例如为了两个有边框的元素排列在一行，需要准确地算出各个元素的宽度，而宽度
都是百分数，边框却是像素值。不同单位，很难得出结果，但有了calc()函数，结果就能手到擒来。
```

* 执行下面的代码，经过计算后，`p`元素的真实行高为多少？

```css
div {
    font-size: 18px;
    line-height:14px;
}
div p {
    line-height: 50%;  /* 9px */
}

/*
行高参照的是元素自身的字体大小(font-size)，p元素自身没有定义字体，需要从父元素div中继承过来，继承过来的值为18px，再乘以50%，
可以得到p元素的最终行高9px。
*/
```

* `line-height: 1.5`与`line-height: 150%`的区别

```
区别体现在子元素继承时，如下：
* line-height: 1.5; 会直接继承给子元素，子元素根据自己的font-size再去计算子元素的line-height
* line-height: 150%; 是计算好了line-height值，然后把这个计算值给子元素继承，子元素拿到的就是最终值。此时，子元素设置
  font-size就对其line-height无影响。
```

* `CSS`中使用`background: transparent`与`opacity: 0`有什么区别？

```
在CSS中，transparent关键字相当于rgba(0, 0, 0, 0)，作为background的属性值，仅仅是将元素的背景设为透明，元素中的内容还能显示。
而opacity会把元素和内容当成一个整体，当定义为0时，两者都会透明。
```

* 执行下面代码，虽然给`p`元素设置了`15px`上外边距，但为何失效？

```css
<style>
div {
    float: left;
    width: 80px;
    height: 80px;
}
p {
    clear: both;
    margin-top: 15px;
}
</style>
<div></div>
<p>已设置上外边距</p>

/*
浮动元素会脱离正常流，clear属性会让元素增加上外边距，使其在浮动元素的下面，在上面的代码中，浮动元素的高是80px，所以clear属性
会给p元素增加80px的上外边距，比定义的15px要大，所以最终的上外边距是80px，正好在浮动元素的下面。
*/
```

* 如何用纯`CSS`的方式让超出容器宽度的文本自动替换为省略号？

```css
/*
可以使用text-overflow属性，这个属性用于显示内容溢出时的省略标记，例如内容太多，将超出部分替换为省略号(...)。但要实现这个效果，
需要满足3个条件，那就是容器要有明确的宽度，强制在一行显示以及隐藏溢出内容。
*/
p {
    width: 200px;             /*容器有明确宽度*/
    white-space: nowrap;      /*强制在一行显示*/
    overflow: hidden;         /*溢出隐藏*/
    text-overflow: ellipsis;  /*溢出内容替换为省略号*/
}
/*
text-overflow有三个值：分别是两个关键字（clip和ellipsis）和一个字符串
* clip：将溢出的内容裁剪掉
* ellipsis: 将溢出内容替换为省略号
* 字符串： 自定义一个省略标记，但这个属性值兼容性不理想，慎用。
*/
```

* 背景图像可以用`Data URI`描述，那么什么是`Data URI`？

```
Data URI可以将外部资源（例如图像），经过Base64编码后，嵌入到其他文档中，可减少额外的HTTP请求。
虽然使用Data URI减少了一次HTTP请求，但它会让嵌入的文档体积膨胀，影响浏览器渲染，并且还会降低Gzip的压缩效率，破坏资源的缓存。
```

* 过渡与动画的区别是什么？

```
过渡与动画有3个方面的不同：
1.过渡只能指定元素的初始状态和结束状态，而动画通过关键帧，可以控制变化过程中的更多状态
2.动画不需要触发条件，当HTML文档和相关样式载入完成后，就能立即执行
3.动画的子属性比过渡多，可以控制动画的循环次数、播放方向以及动画状态
```



