# BFC\(Block Formatting Context\)

## 什么是BFC

BFC全称为Block Formatting Context，即块格式化上下文。块格式化上下文是页面CSS视觉渲染的一部分，用于决定块盒子的布局及浮动相互影响范围的一个区域。

## BFC的创建方法

1. 根元素或其他包含它的元素；
2. 浮动（元素的float不为none）；
3. 绝对定位元素（元素的position为absolute或fixed）；
4. 行内块（display: inline-block）；
5. 表格单元格（display: table-cell，HTML表格单元格默认属性）；
6. overflow的值不为visible的元素；
7. 弹性盒（display: flex 或 inline-flex）；

其中，最常见的为overflow: hidden、float: left/right、position: absolute。

## BFC的效果

1. 内部的盒会在垂直方向一个接一个排列（看作常规流）；
2. 处于同一个BFC中元素相互影响，可能会发生margin崩塌；
3. 每个元素的margin box的左边，与容器块border box的左边想接触，浮动也是如此；
4. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素；
5. 计算BFC高度时，考虑BFC所包含的所有元素，浮动也要参与计算；
6. 浮动盒区域不叠加在BFC上。

## 实际代码演示

```css
<style>
    * {
        margin: 0;
        padding: 0;
    }
    .box {
        background:#000;
        height: 100%;
        margin-left: 50px;
    }
    .left {
        background: red;
        border: 3px solid #F31264;
        width: 200px;
        height: 200px;
        float: left;
    }
    .right {                        
        background: green;
        border: 3px solid #F31264;
        width:400px;
        min-height: 100px;
    }
</style>

<div class="box">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

显示效果：![](/assets/BFC1.png)可以看出，\('.left'\)红色盒子通过左浮动，脱离了原本的位置，\('.right'\)绿色被定位到黑色父元素左上角，与浮动红盒子发生重叠。

同时，黑盒未创建BFC，因此在计算高度的时候没有考虑到红盒区域，发生了高度坍塌。

给黑盒设置overflow: hidden创建BFC：

```
.BFC {
    	overflow: hidden;
}

<div class="box BFC">
    <div class="left"></div>
    <div class="right"></div>
</div>
```

![](/assets/BFC2.png)黑盒创建一个新的BFC后，计算高度时将红盒区域也考虑进去了。

为绿盒创建BFC：

```
<div class="box BFC">
    <div class="left"></div>
    <div class="right BFC"></div>
</div>
```

![](/assets/BFC3.png)一旦绿盒创建了新的BFC后就不与红盒发生重叠。

## 总结

在实际中，利用BFC可以闭合浮动，防止与浮动元素重叠，也可以利用BFC包含一个元素，防止这个元素与BFC外的元素发生margin坍塌。

