# 左右定宽，中间自适应

题目：假设高度已知，请写出三栏布局，其中左栏、右栏宽度各为 300px，中间自适应。

- 浮动：比较好的兼容性，需要清除浮动（若中间高度不固定，则需要创建 BFC）
- 定位：布局脱离文档流，实用性较差（若中间高度不固定，则不能使用）
- flex 布局：解决上述缺点，移动端使用 flex 较多，ie8 不支持（若中间高度不固定，可以继续使用）
- table 布局：兼容性好，对 SEO 不够友好，当其中一个表格高度变高，其他表格也会变高
- grid 布局：代码量简洁（若中间高度不固定，则不能使用）

浮动方式：

```css
<section class="layout-left-center-right">
    <style>
        .float .left {
            float: left;
            width: 300px;
            height: 100px;
            background-color: red;
        }
        .float .right {
            float: right;
            width: 300px;
            height: 100px;
            background-color: blue;
        }
        .float .center {
            height: 100px;
            background-color: yellow;
        }

    </style>
    <article class="layout float">
        <div class="left"></div>
        <div class="right"></div>
        <div class="center">
            <h1>浮动布局</h1>
            <p>使用浮动布局实现三栏布局</p>
            <p>使用浮动布局实现三栏布局</p>
            <p>使用浮动布局实现三栏布局</p>
        </div>
    </article>
</section>
```

定位方式：

```css
<section class="layout-left-center-right">
    <style>
        .position {
            position: relative;
        }
        .position .left {
            position: absolute;
            left: 0;
            width: 300px;
            height: 100px;
            background-color: red;
        }
        .position .center {
            position: absolute;
            left: 300px;
            right: 300px;
            height: 100px;
            background-color: yellow;
        }
        .position .right {
            position: absolute;
            right: 0;
            width: 300px;
            height: 100px;
            background-color: blue;
        }
    </style>
    <article class="layout position">
        <div class="left"></div>
        <div class="center">
            <h1>定位</h1>
            <p>使用定位实现三栏布局</p>
            <p>使用定位实现三栏布局</p>
            <p>使用定位实现三栏布局</p>
        </div>
        <div class="right"></div>
    </article>
</section>
```

flex 布局方式：

```css
<section class="layout-left-center-right">
    <style>
        .flex {
            display: flex;
        }
        .flex .left {
            width: 300px;
            background-color: red;
        }
        .flex .center {
            flex: 1;
            background-color: yellow;
        }
        .flex .right {
            width: 300px;
            background-color: blue;
        }
    </style>
    <article class="layout flex">
        <div class="left"></div>
        <div class="center">
            <h1>flex布局</h1>
            <p>使用flex布局实现三栏布局</p>
            <p>使用flex布局实现三栏布局</p>
            <p>使用flex布局实现三栏布局</p>
        </div>
        <div class="right"></div>
    </article>
</section>
```

table 布局方式：

```css
<section class="layout-left-center-right">
    <style>
        .table {
            display: table;
            width: 100%;
        }
        .table>div {
            display: table-cell;
        }
        .table .left {
            width: 300px;
            background-color: red;
        }
        .table .center {
            background-color: yellow;
        }
        .table .right {
            width: 300px;
            background-color: blue;
        }
    </style>
    <article class="layout table">
        <div class="left"></div>
        <div class="center">
            <h1>table布局</h1>
            <p>使用table布局实现三栏布局</p>
            <p>使用table布局实现三栏布局</p>
            <p>使用table布局实现三栏布局</p>
        </div>
        <div class="right"></div>
    </article>
</section>
```

grid 布局方式：

```css
<section class="layout-left-center-right">
    <style>
        .grid {
            display: grid;
            grid-template-rows: 100px;
            grid-template-columns: 300px auto 300px;
        }
        .grid .left {
            background-color: red;
        }
        .grid .center {
            background-color: yellow;
        }
        .grid .right {
            background-color: blue;
        }
    </style>
    <article class="layout grid">
        <div class="left"></div>
        <div class="center">
            <h1>grid布局</h1>
            <p>使用grid布局实现三栏布局</p>
            <p>使用grid布局实现三栏布局</p>
            <p>使用grid布局实现三栏布局</p>
        </div>
        <div class="right"></div>
    </article>
</section>
```

效果演示：![](/assets/左右定宽中间自适应.png)
