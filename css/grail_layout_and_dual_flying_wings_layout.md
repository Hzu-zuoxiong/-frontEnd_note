# 圣杯布局&双飞翼布局

圣杯布局和双飞翼布局作为经典的三栏式布局是面试中的常客。两种布局达到的效果上基本相同，都是**两边两栏宽度固定，中间栏宽度自适应。在HTML结构上中间栏在最前面保证了最先渲染中间以提升性能**，并且兼容性良好。

## 圣杯布局

### 思路一：

1. `main`设置`width: 100%;`让它占满窗口，才能有自适应效果
2. 为了形成一行三栏的布局，给三个`div`设置`float: left;`（为父元素创建BFC，否则会造成父元素高度塌陷）
3. 利用负`margin`将`left`块放置左侧，将`right`块放置右侧
   1. `left`块较为特殊，需要设置为`margin: -100%;`因为`margin`的百分比是相对于父元素的，所以需要整整一行的宽度才能补偿`margin`值，才能将`left`块移动到`main`块的左边
   2. `right`块则相对简单些，只需要将其左移`right`块的宽度，即可将`right`块移动到`main`块的右侧
4. 此时，已完成将`left`块和`right`块放置在`main`块两侧，但此时的`left`块与`right`块实际上是“压”在`main`块的上面，所以需要中间栏给两边的位置腾出来。为父元素加上`padding`将左右两侧位置腾出。
5. 接着为`left`块与`right`块设置`position: relative;`移动左右两栏，使其不遮挡`main`块
   1. `left`块设置`left: -200px;`
   2. `right`块设置`left: 200px;`

圣杯布局局限：存在一个最小宽度，当页面小于最小宽度时布局就会乱掉。所以最好给`body`设置一个`min-width`，`min-width = leftWidth * 2 + rightWidth`。原因：**因为设置了相对定位，所以当left块原来的位置和right块的位置产生重叠时，由于浮动的原因一行放不下就会换行，布局就被打乱了**。

### 思路二：

1. 前面四步与思路一一样，第五步的解决方法与思路一不同。
2. 将`main`块的盒子模型设置为`border-box;`（设置为`border-box`，`content`的宽则为`width-padding-border`）
3. 然后再为`main`块设置左右`padding`，即可为`left`块与`right`块腾出两侧的位置

### 思路一实现代码：

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    header {
        text-align: center;
        background-color: #eee;
    }
    footer {
        text-align: center;
        background-color: #eee;
    }
    .bg {
        overflow: hidden;          /*创建BFC*/
        padding: 0 200px;
        background-color: #999;
    }
    .bg .text {
        float: left;
        text-align: center;
        line-height: 300px;
        font-size: 64px;
        height: 300px;
    }
    .main {
        width: 100%;
        background-color: red;
    }
    .left {
        position: relative;
        left: -200px;
        width: 200px;
        margin-left: -100%;
        opacity: 0.6;
        background-color: blue;
    }
    .right {
        position: relative;
        left: 200px;
        width: 200px;
        margin-left: -200px;
        opacity: 0.6;
        background-color: yellow;
    }
    </style>
</head>
<body>
    <header>圣杯布局</header>
    <div class="bg">
        <div class="main text">main main main main</div>
        <div class="left text">left</div>
        <div class="right text">right</div>
    </div>
    <footer>footer</footer>
</body>
</html>
```

### 思路二实现代码：

```css
/*省略部分相同的代码*/
<style>
.main {
    box-sizing: border-box;
    padding: 0 200px;
    width: 100%;
    background-color: red;
}
.left {
    width: 200px;
    margin-left: -100%;
    opacity: 0.6;
    background-color: blue;
}
.right {
    width: 200px;
    margin-left: -200px;
    opacity: 0.6;
    background-color: yellow;
    }
</style>
/*省略部分相同的代码*/
```

### 效果如下：![](/assets/圣杯布局.png)

## 双飞翼布局

### 思路：

1. 前面的步骤与圣杯布局相差无几，只是`HTML`结构需要稍微修改以下，在`main`里面添加一个内容层。因为`main`块已经设置了`width: 100%;`故我们不能直接给`main`块添加`margin`属性。故添加一个`main-content`块，将`main`块的内容放入到`main-content`中，再给`main-content`设置`margin`。
2. 为`main-content`设置`margin: 0 200px 0 200px;`就可以完成双飞翼布局。

### 实现代码：

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    header {
        text-align: center;
        background-color: #eee;
    }
    footer {
        text-align: center;
        background-color: #eee;
    }
    .bg {
        overflow: hidden;
        background-color: #999;
    }
    .bg .text {
        float: left;
        text-align: center;
        line-height: 300px;
        font-size: 64px;
        height: 300px;
    }
    .main {
        width: 100%;
        background-color: red;
    }
    .main-content {
        margin: 0 200px 0 200px;
    }
    .left {
        width: 200px;
        opacity: 0.6;
        margin-left: -100%;
        background-color: blue;
    }
    .right {
        width: 200px;
        opacity: 0.6;
        margin-left: -200px;
        background-color: yellow;
    }
    </style>
</head>
<body>
    <header>双飞翼布局</header>
    <div class="bg">
        <div class="main text">
            <div class="main-content">main</div>
        </div>
        <div class="left text">left</div>
        <div class="right text">right</div>
    </div>
    <footer>footer</footer>
</body>
</html>
```

### 效果如下：![](/assets/双飞翼布局.png)

## 两种布局的区别

* 圣杯布局是中间栏为两边腾开位置

![](/assets/圣杯&双飞翼1.png)

* 双飞翼布局是中间栏不变，存放内容的部分为两边腾开位置

![](/assets/圣杯&双飞翼2.png)

参考连接：[https://juejin.im/entry/5a8868cdf265da4e7e10c133](https://juejin.im/entry/5a8868cdf265da4e7e10c133)

