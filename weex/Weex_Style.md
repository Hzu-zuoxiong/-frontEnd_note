# `Weex` 样式

## 通用样式

### 盒模型

&emsp;&emsp;`Weex` 盒模型基于 CSS 盒模型，每个 `Weex` 元素都可视作一个盒子。

**注意：**`Weex` 盒模型的 `box-sizing` 默认为 `border-box`，即盒子的宽高包含内容、内边距和边框的宽度，不包含外边距的宽度。

> **警告** `overflow: hidden;` 在 `Android` 上是默认行为，但只有下列条件都满足时，一个父 `view` 才会去 `clip` 它的子 `view`。这个限制只对 `Android` 生效，`iOS` 不受影响。
>
> - 父 `view` 是 `div, a, cell, refresh` 或 `loading`。
> - 系统版本是 `Android 4.3` 或更高。
> - 系统版本不是 `Android 7.0`。
> - 父 `view` 没有 `background-image` 属性或系统版本是 `Android 5.0` 或更高。

### `Flexbox`

&emsp;&emsp;`Weex` 布局模型基于 `CSS Flexbox`，以便所有页面元素的排版能够一致可预测，同时页面布局能适应各种设备或屏幕尺寸。在 `Weex` 中，`Flexbox` 是默认且唯一的布局模型，所有不需要手动为元素添加 `display: flex;` 属性。

### 定位

&emsp;&emsp;`Weex` 支持 `position` 定位，用法与 `CSS position` 类似。为元素设置 `position` 后，可通过 `top, right, bottom, left` 四个属性设置元素坐标。

`position` : 可选值为:

- `relative`：是默认值，指的是相对定位
- `absolute`：是绝对定位，以元素的容器座位参考系
- `fixed`：保证元素在页面窗口中的对应位置显示
- `sticky`：指的是仅当元素滚动到页面之外时，元素会固定在页面窗口的顶部

**注意：**

1. `Weex` 目前不支持 `z-index` 设置元素层级关系，但靠后的元素层级更高，因此，对于层级高的元素，可将其排列在后面。
2. 如果定位元素超过容器边界，在 `Android` 下，超出部分将不可见，原因在于 `Android` 端元素 `overflow` 默认值为 `hidden`，但目前 `Android` 暂不支持设置 `overflow: visible`。

### `transform`

&emsp;&emsp;`transform` 属性向元素应用 `2D` 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

目前支持的 `transform` 声明格式：

- `translate`
- `translateX`
- `translateY`
- `scale()`
- `scaleX()`
- `scaleY()`
- `rotate()`
- `rotateX()`
- `rotateY()`
- `perspective() Android` 4.1 及以上版本支持
- `transform-origin: number/percentage/keyword(top/left/right/bottom)`

### `transition`

&emsp;&emsp;可以在 `CSS` 中使用 `transition` 属性来提升您应用的交互性与视觉感受，`transition` 中包括布局动画，即 `LayoutAnimation`，现在布局产生变化的同时也能使用 `transition` 带来的流畅动画。`transition` 允许 `CSS` 的属性值在一定的时间区间内平滑地过渡。

### 伪类

&emsp;&emsp;`Weex` 支持四种伪类： `active, focus, disabled, enabled`

&emsp;&emsp;所有组件都支持 `active`，但只有 `input` 和 `textarea` 组件支持 `focus, enabled, disabled`

### 线性渐变

&emsp;&emsp;`Weex` 支持线性渐变背景，所有组件均支持线性渐变。可以通过 `background-image` 属性创建线性渐变。目前暂不支持 `radial-gradient`（径向渐变）

```
background-image: linear-gradient(to top, #a80077, #66ff00);
```

`Weex` 目前只支持两种颜色的渐变，渐变方法如下：

- `to right`：从左向右渐变
- `to left`：从右向左渐变
- `to bottom`：从上向下渐变
- `to top`：从下向上渐变
- `to bottom right`：从左上角向右下角
- `to top left`：从右下角向左上角

**注意：**

- `background-image` 优先级高于 `background-color`
- 不要使用 `background` 简写。

### 阴影（`box-shadow`）

&emsp;&emsp;`Weex` 支持阴影属性： `active, focus, disabled, inset(可选), offset-x, offset-y, blur-radius, color`。`box-shadow` 仅仅支持 `IOS`

### 文本样式

&emsp;&emsp;文本类组件共享一些通用样式。

- `color`： 文字颜色。不支持 `hsl(), hsla(), currentColor`。`6-chars hex(十六进制)` 是性能最好的颜色使用方式。除非有特殊原因，请使用 `6-chars hex` 格式。
  - RGB(`rgb(255, 0, 0)`)
  - RGBA(`rgba(255, 0, 0, 0.5)`)
  - 十六进制(`#ff0000`)；精简法十六进制(`#f00`)
  - 色值关键字(`red`)
- `font-size`: 文字大小
- `font-style`：字体类别。可选值 `normal` | `italic`，默认为 `normal`
- `font-weight`：字体粗细程度
  - 可选值： `normal, bold, 100, 200, 300, 400, 500, 600, 700, 800, 900`
  - `normal` 等同于 `400`， `bold` 等同于 `700`
  - 默认值：`normal`
  - `IOS` 支持 `9` 种 `font-weight` 值；`Android` 仅支持 `400` 和 `700`，其他值会设为 `400` 和 `700`
  - 类似 `lighter, bolder` 这样的值暂不支持
- `text-decoration`：字体修饰。可选值 `none` | `uderline` | `line-through`，默认为 `none`。只有 `<text>` 和 `<richtext>` 组件支持
- `text-align`：对齐方式。可选值 `left` | `center` | `right`，默认值为 `left`。暂不支持 `justify, justify-all`
- `font-family`：设置字体。这个设置不保证在不同平台，设备间的一致性。如所选设置在平台上不可用，将会降级到平台默认字体
- `text-overflow`：设置内容超长时的省略样式。可选值 `clip` | `ellipsis`。只有 `<text>` 和 `<richtext>` 组件支持
- `lines`：正整数，指定最大文本行数，默认为 0，表示不限制最大行数。如果文本不够长，实际展示行数会小于指定行数。
- `line-height`：正整数，每行字高度。`line-height` 是 `top` 至 `bottom` 的举距离。 `line-height` 与 `font-size` 没有关系，因为 `line-height` 被 `top` 和 `bottom` 所限制，`font-size` 被 `glyph` 所解析。`line-height` 和 `font-size` 相等一般会导致文字被截断。
