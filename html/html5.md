# `HTML5`新特性

`HTML5`添加了很多新元素及功能，比如：图形的绘制，多媒体内容，更好的页面结构，更好的形式处理，和几个拖放`API`，定位，包括页面应用程序缓存，存储，网络工作者等。

## 语义标签

| 标签                              | 描述                             |
| :-------------------------------- | :------------------------------- |
| &lt;hrader&gt;&lt;/header&gt;     | 定义文档的头部区域               |
| &lt;footer&gt;&lt;/footer&gt;     | 定义文档的尾部区域               |
| &lt;nav&gt;&lt;/nav&gt;           | 定义文档的导航                   |
| &lt;section&gt;&lt;/section&gt;   | 定义文档中的节（section、区段）  |
| &lt;article&gt;&lt;/article&gt;   | 定义页面独立的内容区域           |
| &lt;aside&gt;&lt;/aside&gt;       | 定义页面的侧边栏内容             |
| &lt;detailes&gt;&lt;/detailes&gt; | 用于描述文档或文档某个部分的细节 |
| &lt;summary&gt;&lt;/summary&gt;   | 标签包含 details 元素的标题      |
| &lt;dialog&gt;&lt;/dialog&gt;     | 定义对话框，比如提示框           |

## 增强型表单

`HTML5` 拥有多个新的表单输入类型，提供了更好的输入验证。

| 输入类型       | 描述                          |
| :------------- | :---------------------------- |
| color          | 主要用于选取颜色              |
| date           | 从一个日期选择器选择一个日期  |
| datetime       | 选择一个日期（UTC 时间）      |
| datetime-local | 选择一个日期和时间 \(无时区\) |
| email          | 包含 e-mail 地址的输入域      |
| month          | 选择一个月份                  |
| number         | 数值的输入域                  |
| range          | 一定范围内数字值的输入域      |
| search         | 用于搜索域                    |
| tel            | 定义输入电话号码字段          |
| time           | 选择一个时间                  |
| url            | URL 地址的输入域              |
| week           | 选择周和年                    |

`HTML5` 新增以下表单元素

| 表单元素         |                                             描述                                              |
| :--------------- | :-------------------------------------------------------------------------------------------: |
| &lt;datalist&gt; | 元素规定输入域的选项列表使用 &lt;input&gt; 元素的 list 属性与 &lt;datalist&gt; 元素的 id 绑定 |
| &lt;keygen&gt;   |               提供一种验证用户的可靠方法，标签规定用于表单的密钥对生成器字段。                |
| &lt;output&gt;   |                            用于不同类型的输出，比如计算或脚本输出                             |

`HTML5` 新增的表单属性

- `placehoder`属性：简单的提示，在用户输入值前显示在输入域上，即常见的输入框默认提示。
- `required`属性：是一个`boolean`属性。要求填写的输入域不能为空。
- `pattern`属性：描述了一个正则表达式用于验证`<input>`元素的值。
- `min`和`max`属性：设置元素最小值与最大值。
- `step`属性：为输入域规定合法的数字间隔。
- `height`和`width`属性：用于`image`类型的`<input>`图像高度和宽度。
- `autofocus`属性：`boolean`属性。规定在页面加载时，域自动地获得焦点。
- `multiple`属性：`boolean`属性。规定`<input>`元素中可选择多个值。

## 视频和音频

- `HTML5` 提供了播放音频文件的标准，即`<audio>`元素

```
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
  您的浏览器不支持 audio 元素。
</audio>
```

`control`属性供添加播放、暂停和音量控件。在`<audio></audio>`之间插入浏览器不支持的`<audio>`提示文本。

`<audio>`元素允许使用多个&lt;source&gt;元素，链接不同的音频文件，浏览器将使用第一个支持的音频文件。目前支持三种音频格式：`.mp3`， `.wav`和`.opp`。

- `HTML5`提供了播放视频的标准，即`<video>`元素

```
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  您的浏览器不支持Video标签。
</video>
```

`control`提供了播放、暂停和音量控件，也可以使用`dom`操作来控制视频的播放暂停，如`play()`和`pause()`方法。`<video></video>`之间插入浏览器不支持的`<video>`提示文本。同时 video 元素提供了`width`和`height`属性控制视频的尺寸，若设置了高度和宽度，所需视频空间在页面加载时保留，若没有设置，则浏览器在加载时不能保留特定的空间，页面会根据原始视频的大小改变。

`<video>`元素支持多个`source`元素，元素可以链接不同的视频文件，浏览器将使用第一个可识别的格式（`.mp4`，`.webm`和`.ogg`）。

## Canvas 绘图

标签只是图形容器，必须使用脚本来绘制图形。

### Canvas - 图形

- 创建一个画布，画布在网页中是一个矩形框。默认没有边框和内容。

```
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000;"></canvas>
```

- 使用`JavaScript`绘制图像，`Canvas`元素本身没有绘图能力，所有绘制工作必须在`JavaScript`内部完成。

```js
var ctx = document.getElementById('myCanvas').getContext('2d');
ctx.fillStyle = '#F00';
ctx.fillRect(0, 0, 150, 75);
```

`getContext("2d")`对象是内建的`HTML5`对象，拥有多种绘制路径、矩形、圆形、字符等方法。

### Canvas - 文本

使用`canvas`绘制文本，重要的属性和方法：

- `font`：定义字体
- `fillText(text, x, y)`：在`canvas`上绘制实心的文本
- `strokeText(text, x, y)`：在`canvas`上绘制空心的文本

```js
var ctx = document.getElementById('myCanvas').getContext('2d');
ctx.font = '30px Arial';
ctx.fillText('Hello World', 10, 50);
```

### Canvas - 渐变

渐变可以填充在矩形、圆形、线条、文本等等，各种形状可以定义不同的颜色。

- `createLinearGradient(x, y, x1, y1)`--创建线条渐变

- `createRadialGradient(x, y, r, x1, y1, r1)`--创建一个径向/园渐变

当使用渐变对象，必须使用两种或两种以上的停止颜色。`addColorStop()`方法指定颜色停止，参数使用坐标描述，可以是 0 到 1。

使用渐变，设置`fillStyle`或`strokeStyle`的值为渐变，然后绘制形状。

```js
var ctx = document.getElementById('myCanvas').getContext('2d');

// Create gradient
var grd = ctx.createLinearGradient(0, 0, 200, 0);
grd.addColorStop(0, 'red');
grd.addColorStop(1, 'white');

// Fill with gradient
ctx.fillStyle = grd;
ctx.fillRect(10, 10, 150, 80);
```

### Canvas - 图像

把一副图像放置到画布上，使用`drawImage(image, x, y)`方法

```js
var ctx = document.getElementById('myCanvas').getContext('2d');
var img = document.getElementById('scream');
ctx.drawImage(img, 10, 10);
```

## SVG 绘图

`SVG`是一种使用`XML`描述`2D`图形的语言。`Canvas`则是通过`JavaScript`绘制 2D 图形。`SVG`基于`XML`，则`SVG DOM`中的每个元素都是可用的，可以为某个元素附加事件处理。在`SVG`中，每个被绘制的图形均被视为对象。如果`SVG`对象的属性发生变化，那么浏览器能够自动重现图形。

## 拖放 API

拖放是一种常见的特性。在`HTML5`中，拖放是标准的一部分，任何元素都能拖放。拖放的过程分为源对象和目标对象。源对象是指即将拖动元素，而目标对象则是拖动之后要放置的目标位置。

拖放的源对象可触发 3 个事件：`dragstart`（拖放开始）、`drag`（拖放中）、`dragend`（拖放结束）。

拖放的目标对象可触发 4 个事件：`dragenter`（拖放进入）、`dragover`（拖放悬停）、`dragleave`（拖放离开）、`drop`（释放）

`dataTransfer`：用于数据传递的“拖拉机”对象：

在拖动数据源对象中使用`e.dataTransfer`属性保存数据：`e.dataTransfer.setData(k, v)`

在拖动目标对象事件中使用`e.dataTransfer`属性读取数据：`e.dataTransfer.getData(k, v)`

## Web Worker

`web Worker`是运行在后台的`JavaScript`，独立于其他脚本，不会影响页面的性能。可以继续做其他事情：点击、选取内容等，而此时`web Worker`在后台运行。

```js
// 检测是否支持web Worker
if (typeof Worker !== 'undefined') {
  // 是的! Web worker 支持!
  // 一些代码.....
} else {
  // //抱歉! Web Worker 不支持
}

// 检测是否存在worker
if (typeof w == 'undefined') {
  w = new Worker('demo_workers.js');
}

// 从web Worker发送和接受信息。
w.onmessage = function(event) {
  document.getElementById('result').innerHTML = event.data;
};
```

如果需要终止`web Worker`，并释放浏览器资源，使用`terminate()`方法。

## Web Storage

使用`HTML5`可以在本地存储用户的浏览数据，数据以键值对存在。

客户端存储数据的两个对象：

- `localStorage`：没有时间限制的数据存储。
- `seeionStorage`：针对一个`session`的数据存储，当用户关闭浏览器窗口后，数据会被删除。

`localStorage`和`seesionStorage`可使用的`API`都相同，常用的有：（以`localStorage`为例）

- 保存数据：`localStorage.setItem(key,value);`
- 读取数据：`localStorage.getItem(key);`
- 删除单个数据：`localStorage.removeItem(key);`
- 删除所有数据：`localStorage.clear();`
- 得到某个索引的`key`：`localStorage.key(index);`

## WebSocket

`WebSocket`是`HTML5`提供的一种在单个`TCP`连接上进行全双工通讯的协议。在`WebSocket API`中，浏览器和服务器只需要做一个握手动作，则浏览器和服务器之间就形成了一条快速通道。两者之间可以实现数据互相传送。浏览器通过`JavaScript`向服务器发出建立`webSocket`链接的请求，连接建立后，客户端和服务器端可以通过 TCP 连接直接交换数据。当获取`web socket`连接后，可以通过`send()`方法向服务器发送数据，通过`onmessage`事件接受服务器返回的数据。
