# `Weex` 事件

## 通用事件

### `click`

&emsp;&emsp;当组件上发生点击手势时被触发。`<input>` 组件目前不支持 `click` 事件，使用 `change` 或 `input` 事件代替。

### `longpress`

&emsp;&emsp;如果一个组件被绑定了 `longpress` 事件，当用户长按这个组件时，该事件将被触发。

### `appear`

&emsp;&emsp;如果一个位于某个可滚动区域内的组件被绑定了 `appear` 事件，当这个组件的状态变为在屏幕上可见时，该事件将被触发。

### `disappear`

&emsp;&emsp;如果一个位于某个可滚动区域内的组件被绑定了 `disappear` 事件，那么当这个组件被滑出屏幕变为不可见状态时，该事件将被触发。

### `Page`

&emsp;&emsp;**注：**仅支持 `IOS` 和 `Android`，`H5` 暂不支持。

&emsp;&emsp;`Weex` 通过 `viewappear` 和 `viewdisappear` 事件提供了简单的页面状态管理能力。`viewappear` 事件会在页面就要显示或配置的任何页面动画被执行前触发，例如，当调用 `navigator` 模块的 `push` 方法时，该事件将会在打开新页面时被触发。`viewdisappear` 事件会在页面就要关闭时被触发。

&emsp;&emsp;与组件的 `appear` 和 `disappear` 事件不同的是，`viewappear` 和 `viewdisappear` 事件关注的是整个页面的状态，所以它们必须绑定到页面的根元素上。特殊情况下，这两个事件也能被绑定到非根元素的 `body` 组件上，例如 `wxc-navpage` 组件。

## 事件冒泡

`Weex` 在 `0.13` 之前是不支持这一特性的，在 `0.13` 版本，`Weex` 提供了这项支持。

> 注意： 目前仅 `Weex Native`（`Android` 和 `iOS`）支持，`web` 端 暂时不支持此项特性。事件冒泡默认是不开启的，你需要在模板根元素上加上属性 `bubble=true` 才能开启冒泡。

### 使用

事件冒泡默认不开启，需手动添加 `bubble=true` 属性到根元素上。

```
<template>
  <!-- 开启事件冒泡机制. -->
  <div bubble="true">
    ...
  </div>
</template>
```

### 阻止冒泡

在事件处理函数里，可以通过调用 `event.stopPropagation` 方法阻止事件冒泡。这个方法和 `DOM` 标准里的方法一致。注意 `event.stopPropagation` 和 `bubble=true` 的影响范围不同，前者仅针对当前冒泡到的元素以及其祖先层级有效，而对子元素无效。而后者相当于一个全局开关，开启以后对该根元素内部所有子元素都开启了事件冒泡效果。两者可以同时存在。

```
{
  handleClick (event) {
    // 阻止继续冒泡.
    event.stopPropagation()
  }
}
```

## 手势（属于实验性功能）

### 手势类型

目前，仅支持以下四种手势类型：

* `Touch`：当触摸到一个点，移动或从触摸面移开时触发 `touch` 手势。触摸手势很精准，它会返回所有详细的事件信息。所以，监听 `touch` 手势可能很慢，即使只移动一丁点也需要处理大量事件。有三种类型的 `touch` 手势：

  * `touchstart` 将在触摸到触摸面上时触发。
  * `touchmove` 将在触摸点在触摸面移动时被触发。
  * `touchend` 将在从触摸面离开时被触发。
  * `stopPropagation` 每个 `touch` 事件都会被传递过来, 可控制 `touch` 事件是否冒泡（返回 `true` ）或者停止（返回 `false` ）；用于解决事件冲突或者自定义手势。

* `Pan`：`pan` 手势也会返回触摸点在触摸面的移动信息，有点类似于 `touch` 手势。但是 `pan` 手势只会采样收集部分事件信息因此比 `touch` 事件要快得多，当然精准性差于 `touch`。`pan` 也有三种类型的手势，这些手势的意义与 `touch` 完全一样：

  * `panstart`
  * `panmove`
  * `panend`
  * `horizontalpan`：手势的 `start/move/end` 状态保存在 `state` 特性中。目前该手势在 `Android` 下会与 `click` 事件冲突。
  * `verticalpan`：手势的 `start/move/end` 状态保存在 `state` 特性中。目前该手势在 `Android` 下会与 `click` 事件冲突。

* `Swipe`：`swipe` 将会在用户在屏幕上滑动时触发，一次连续的滑动只会触发一次 `swiper` 手势。

* `LongPress`：`LongPress` 将会在触摸点连续保持 `500 ms` 以上时触发。

`touch` 和 `pan` 非常接近，它们的特点可以总结成这样：

* `Touch`：完整信息，精准、很慢
* `Pan`：抽样信息，很快，不够精准

开发者可以根据自己的情况选择合适的手势。

### 属性

以下属性可以在手势的回调中使用：

* `direction`：仅在 `swipe` 手势中存在，返回滑动方向，返回值可能为 `up, left, bottom, right`。
* `changedTouches`：一个数组，包含了当前手势的触摸点的运动轨迹

**`changedTouches`**

`changedTouches` 是一个数组，其子元素中包含以下属性：

* `identifier`：触摸点的唯一标识符。
* `pageX`：触摸点相对于文档左侧边缘的 X 轴坐标。
* `pageY`：触摸点相对于文档顶部边缘的 Y 轴坐标。
* `screenX`：触摸点相对于屏幕左侧边缘的 X 轴坐标。
* `screenY`：触摸点相对于屏幕顶部边缘的 Y 轴坐标。
* `force`: 屏幕收到的按压力度，值的范围为 0~1

> `force` 属性目前在支持 `forceTouch iOS` 设备才支持, `iPhone 6s` 及更新的 `iOS` 设备

### 约束

目前，由于会触发大量事件冲突，`Weex Android` 还不支持在滚动类型的元素上监听手势，例如 `scroller`, `list` 和 `webview` 这三个组件。