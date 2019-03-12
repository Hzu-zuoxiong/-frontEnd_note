# 上下高度固定，中间自适应

题目：写出三栏布局，其中顶部、底部高度各为 300px，中间自适应。

- 定位布局
- flex 布局

**补充：**

`overflow: auto` 在 `iOS` 上滑动卡顿现象的解决方法：

```css
body {
  -webkit-overflow-scrolling: touch;
  overflow-scrolling: touch;
}
```

## 定位布局：

```css
<section class="layout">
    <style>
        .layout-top-center-bottom>div {
            width: 100%;
            min-height: 100px;
        }
        .top {
            position: absolute;
            top: 0;
            left: 0;
            background-color: red;
        }
        .center {
            position: absolute;
            top: 100px;
            bottom: 100px;
            background-color: yellow;
            overflow: auto;
        }
        .bottom {
            position: absolute;
            bottom: 0;
            background-color: blue;
        }
    </style>
    <article class="layout-top-center-bottom">
        <div class="top"></div>
        <div class="center">
            <h1>上下固定，中间自适应</h1>
            <p>上下固定，中间自适应</p>
            <p>上下固定，中间自适应</p>
            <p>上下固定，中间自适应</p>
            <!-- 若干个p标签 -->
            ....
        </div>
        <div class="bottom"></div>
    </article>
</section>
```

## flex 布局：

```css
<section class="layout">
    <style>
        html,body,.layout {
            height: 100%;
        }
        .layout-top-center-bottom {
            display: flex;
            flex-direction: column;
            height: 100%;
            -webkit-box-orient:vertical;
        }
        .layout-top-center-bottom>div {
            width: 100%;
            min-height: 100px;
        }
        .top {
            background-color: red;
        }
        .center {
            flex: 1;
            overflow: auto;
            background-color: yellow;
        }
        .bottom {
            background-color: blue;
        }
    </style>
    <article class="layout-top-center-bottom">
        <div class="top"></div>
        <div class="center">
            <h1>上下固定，中间自适应</h1>
            <p>上下固定，中间自适应</p>
            <p>上下固定，中间自适应</p>
            <p>上下固定，中间自适应</p>
            <p>上下固定，中间自适应</p>
            <!-- 若干个p标签 -->
            ....
        </div>
        <div class="bottom"></div>
    </article>
</section>
```

## 效果演示：![](/assets/上下高度固定，中间自适应.png)
