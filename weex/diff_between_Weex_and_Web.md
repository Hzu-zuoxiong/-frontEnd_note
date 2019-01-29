# Weex 与 Web 平台的差异

## `Weex`

&emsp;&emsp;`Weex` 表面上是一个客户端技术，但实际上它串联起了从本地开发、云端部署到分发的整个链路。开发者首先可在本地像编写 `web` 页面一样编写一个 `app` 的界面，然后通过命令行工具将之编译成一段 `JavaScript` 代码，生成一个 `Weex` 的 `JS bundle`；同时，开发者可以将生成的 `JS bundle` 部署至云端，然后通过网络请求或预下发的方式加载至用户的移动应用客户端；在移动应用客户端里，`Weex SDK` 会准备好一个 `JavaScript` 执行环境，并且在用户打开一个 `Weex` 页面时在这个执行环境中执行相应的 `JS bundle`，并将执行过程中产生的各种命令发送到 `native` 端进行界面渲染、数据存储、网络通信、调用设备功能及用户交互响应等功能；同时，如果用户希望使用浏览器访问这个界面，那么他可以在浏览器里打开一个相同的 `web` 页面，这个页面和移动应用使用相同的页面源代码，但被编译成适合 `Web` 展示的 `JS Bundle`，通过浏览器里的 `JavaScript` 引擎及 `Weex SDK` 运行起来的。

&emsp;&emsp;`Weex` 是一个跨平台解决方案，`Web` 平台只是其一种运行环境，除此之外还可以在 `Android` 和 `iOS` 客户端中运行。原生开发平台和 `Web` 平台之间的差异，在功能和开发体验上都有一些差异。

## `Weex` 环境中没有 `DOM`

&emsp;&emsp; `DOM`（Document Object Model），即文档对象模型，是 `HTML` 和 `XML` 文档的编程接口，是 `Web` 中的概念。`Weex` 的运行环境以原生应用为主，在 `Android` 和 `IOS` 环境中渲染出来的是原生的组件，不是 `DOM Element`。原生环境中不支持 `Web API`，没有 `Element、Event`、`File` 等对象，不支持选中元素，如 `document.getElementById` 、`document.querySelector` 等；当然也不支持基于 `DOM API`的程序库（如 jQuery）。

## `Weex` 环境中没有 `BOM`

&emsp;&emsp;`BOM`（Browser Object Model），即浏览器对象模型，是浏览器环境为 `javaScript` 提供的接口。`Weex` 在原生端没有并不基于浏览器运行，不支持浏览器提供的 `BOM` 接口。

&emsp;&emsp;`Weex` 没有提供浏览器中的 `window` 和 `screen` 对象，不支持使用全局变量。如果想要获取设备的屏幕或环境信息，可以使用 `WXEnvironment` 变量。

- `WXEnvironment`
  - `weexVersion`：`WeexSDK` 的版本
  - `appName`：应用的名称
  - `appVersion`：应用的版本
  - `platform`：运行平台，可能的值是 `Web、Android、IOS` 之一
  - `osName`：系统的名称
  - `osVersion`：系统版本
  - `deviceWidth`：设备宽度
  - `deviceHeight`：设备高度

&emsp;&emsp;没有 `history 、location 、navigator` 对象

- `history`：保存了当前页面的历史记录，并且提供了前进后退操作。
- `location`：记录了当前页面 URL 相关的信息。
- `navigator`：记录了当前浏览器中的信息。

&emsp;&emsp;这些接口与浏览器自身的实现有关，可以控制页面的前进后退并且获取状态信息。虽然在 `Android` 和 `iOS` 中也有“历史”和“导航”的概念，但是它是用于多个管理视图之间的跳转的。换句话说，在浏览器中执行“前进”、“后退”仍然会处于同一个页签中，在原生应用中“前进”、“后退”则会真实的跳转到其他页面。

## `Weex` 能够调用移动设备原生 `API`

&emsp;&emsp;在 Weex 中能够调用移动设备原生 API，使用方法是通过注册、调用模块来实现。其中有一些模块是 Weex 内置的，如 clipboard 、 navigator 、storage 等。

## 样式

### 通用样式

#### 盒模型

&emsp;&emsp;`Weex` 盒模型基于 CSS 盒模型，每个 `Weex` 元素都可视作一个盒子。

**注意：**

- `Weex` 对于长度值目前只支持像素值，不支持相对单位（`em, rem`）。
- `Weex` 盒模型的 `box-sizing` 默认为 `border-box`，即盒子的宽高包含内容、内边距和边框的宽度，不包含外边距的宽度。

> **警告** `overflow: hidden;` 在 `Android` 上是默认行为，但只有下列条件都满足时，一个父 `view` 才会去 `clip` 它的子 `view`。这个限制只对 `Android` 生效，`iOS` 不受影响。
>
> - 父 `view` 是 `div, a, cell, refresh` 或 `loading`。
> - 系统版本是 `Android 4.3` 或更高。
> - 系统版本不是 `Android 7.0`。
> - 父 `view` 没有 `background-image` 属性或系统版本是 `Android 5.0` 或更高。

#### `Flexbox`

&emsp;&emsp;`Weex` 布局模型基于 `CSS Flexbox`，以便所有页面元素的排版能够一致可预测，同时页面布局能适应各种设备或屏幕尺寸。在 `Weex` 中，`Flexbox` 是默认且唯一的布局模型，所有不需要手动为元素添加 `display: flex;` 属性。

#### 定位

&emsp;&emsp;`Weex` 支持 `position` 定位，用法与 `CSS position` 类似。为元素设置 `position` 后，可通过 `top, right, bottom, left` 四个属性设置元素坐标。

`position` : 可选值为:

- `relative`：是默认值，指的是相对定位
- `absolute`：是绝对定位，以元素的容器座位参考系
- `fixed`：保证元素在页面窗口中的对应位置显示
- `sticky`：指的是仅当元素滚动到页面之外时，元素会固定在页面窗口的顶部

**注意：**

1. `Weex` 目前不支持 `z-index` 设置元素层级关系，但靠后的元素层级更高，因此，对于层级高的元素，可将其排列在后面。
2. 如果定位元素超过容器边界，在 `Android` 下，超出部分将不可见，原因在于 `Android` 端元素 `overflow` 默认值为 `hidden`，但目前 `Android` 暂不支持设置 `overflow: visible`。

#### `transform`

&emsp;&emsp;`transform` 属性向元素应用 `2D` 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

目前支持的 `transform` 声明格式：

* `translate`
* `translateX`
* `translateY`
* `scale()`
* `scaleX()`
* `scaleY()`
* `rotate()`
* `rotateX()`
* `rotateY()`
* `perspective() Android` 4.1及以上版本支持
* `transform-origin: number/percentage/keyword(top/left/right/bottom)`

#### `transition`

&emsp;&emsp;可以在 `CSS` 中使用 `transition` 属性来提升您应用的交互性与视觉感受，`transition` 中包括布局动画，即 `LayoutAnimation`，现在布局产生变化的同时也能使用 `transition` 带来的流畅动画。`transition` 允许 `CSS` 的属性值在一定的时间区间内平滑地过渡。

#### 伪类

&emsp;&emsp;`Weex` 支持四种伪类： `active, focus, disabled, enabled` 

&emsp;&emsp;所有组件都支持 `active`，但只有 `input` 和 `textarea` 组件支持 `focus, enabled, disabled`

#### 线性渐变

&emsp;&emsp;`Weex` 支持线性渐变背景，所有组件均支持线性渐变。可以通过 `background-image` 属性创建线性渐变。目前暂不支持 `radial-gradient`（径向渐变）

```
background-image: linear-gradient(to top, #a80077, #66ff00);
```

`Weex` 目前只支持两种颜色的渐变，渐变方法如下：

* `to right`：从左向右渐变
* `to left`：从右向左渐变
* `to bottom`：从上向下渐变
* `to top`：从下向上渐变
* `to bottom right`：从左上角向右下角
* `to top left`：从右下角向左上角

**注意：**

* `background-image` 优先级高于 `background-color`
* 不要使用 `background` 简写。

#### 阴影（`box-shadow`）

&emsp;&emsp;`Weex` 支持阴影属性： `active, focus, disabled, inset(可选), offset-x, offset-y, blur-radius, color`。`box-shadow` 仅仅支持 `IOS`

#### 文本样式

&emsp;&emsp;文本类组件共享一些通用样式。

* `color`： 文字颜色
  * RGB(`rgb(255, 0, 0)`)
  * RGBA(`rgba(255, 0, 0, 0.5)`)
  * 十六进制(`#ff0000`)；精简法十六进制(`#f00`)
  * 色值关键字(`red`)
* `font-size`: 文字大小
* `font-style`：字体类别。可选值 `normal` | `italic`，默认为 `normal`
* `font-weight`：字体粗细程度
  * 可选值： `normal, bold, 100, 200, 300, 400, 500, 600, 700, 800, 900`
  * `normal` 等同于 `400`， `bold` 等同于 `700`
  * 默认值：`normal`
  * `IOS` 支持 `9` 种 `font-weight` 值；`Android` 仅支持 `400` 和 `700`，其他值会设为 `400` 和 `700`
  * 类似 `lighter, bolder` 这样的值暂不支持
