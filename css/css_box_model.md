# CSS 盒模型

## 标准盒模型和 IE 盒模型的区别

**标准盒模型：**

box-sizing: content-box;（默认）

![](/assets/CSS标准盒模型.png)

**IE 盒模型：**

box-sizing: border-box;

![](/assets/IE盒模型.png)

## JS 如何设置获取盒模型对应的宽和高

- dom.style.width/height //只能获取 CSS 内联样式，外链 CSS 样式获取不到
- dom.currentStyle.width/height //获取渲染之后的宽和高，只有 IE 支持
- dom.getComputedStyle\(dom\).width/height //获取渲染之后的宽和高，所有浏览器支持
- dom.getBoundingClientRect\(\).width/height //计算元素的绝对位置，根据 viewport 左上角确定，会得到 left/top/width/height
