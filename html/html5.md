# HTML5新特性

HTML5添加了很多新元素及功能，比如：图形的绘制，多媒体内容，更好的页面结构，更好的形式处理，和几个拖放API，定位，包括页面应用程序缓存，存储，网络工作者等。

## 语义标签

| 标签 | 描述 |
| :--- | :--- |
| &lt;hrader&gt;&lt;/header&gt; | 定义文档的头部区域 |
| &lt;footer&gt;&lt;/footer&gt; | 定义文档的尾部区域 |
| &lt;nav&gt;&lt;/nav&gt; | 定义文档的导航 |
| &lt;section&gt;&lt;/section&gt; | 定义文档中的节（section、区段） |
| &lt;article&gt;&lt;/article&gt; | 定义页面独立的内容区域 |
| &lt;aside&gt;&lt;/aside&gt; | 定义页面的侧边栏内容 |
| &lt;detailes&gt;&lt;/detailes&gt; | 用于描述文档或文档某个部分的细节 |
| &lt;summary&gt;&lt;/summary&gt; | 标签包含 details 元素的标题 |
| &lt;dialog&gt;&lt;/dialog&gt; | 定义对话框，比如提示框 |

## 增强型表单

HTML5 拥有多个新的表单输入类型，提供了更好的输入验证。

| 输入类型 | 描述 |
| :--- | :--- |
| color | 主要用于选取颜色 |
| date | 从一个日期选择器选择一个日期 |
| datetime | 选择一个日期（UTC 时间） |
| datetime-local | 选择一个日期和时间 \(无时区\) |
| email | 包含 e-mail 地址的输入域 |
| month | 选择一个月份 |
| number | 数值的输入域 |
| range | 一定范围内数字值的输入域 |
| search | 用于搜索域 |
| tel | 定义输入电话号码字段 |
| time | 选择一个时间 |
| url | URL 地址的输入域 |
| week | 选择周和年 |

HTML5 新增以下表单元素

| 表单元素 | 描述 |
| :--- | :---: |
| &lt;datalist&gt; | 元素规定输入域的选项列表使用 &lt;input&gt; 元素的 list 属性与    &lt;datalist&gt; 元素的 id 绑定 |
| &lt;keygen&gt; | 提供一种验证用户的可靠方法，标签规定用于表单的密钥对生成器字段。 |
| &lt;output&gt; | 用于不同类型的输出，比如计算或脚本输出 |

HTML5 新增的表单属性

* placehoder 属性：简单的提示，在用户输入值前显示在输入域上，即常见的输入框默认提示。
* required 属性：是一个boolean 属性。要求填写的输入域不能为空。
* pattern 属性：描述了一个正则表达式用于验证&lt;input&gt;元素的值。
* min和max 属性：设置元素最小值与最大值。
* step 属性：为输入域规定合法的数字间隔。
* height和width 属性：用于image类型的&lt;input&gt;图像高度和宽度。
* autofocus 属性：boolean 属性。规定在页面加载时，域自动地获得焦点。
* multiple 属性：boolean 属性。规定&lt;input&gt;元素中可选择多个值。

## 视频和音频

* HTML5 提供了播放音频文件的标准，即&lt;audio&gt;元素

```
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
  您的浏览器不支持 audio 元素。
</audio>
```

control 属性供添加播放、暂停和音量控件。在&lt;audio&gt;&lt;/audio&gt;之间插入浏览器不支持的&lt;audio&gt;提示文本。

&lt;audio&gt;元素允许使用多个&lt;source&gt;元素，链接不同的音频文件，浏览器将使用第一个支持的音频文件。目前支持三种音频格式：.mp3， .wav和.opp。

* HTML5 提供了播放视频的标准，即&lt;video&gt;元素

```
<video width="320" height="240" controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  您的浏览器不支持Video标签。
</video>
```

control提供了播放、暂停和音量控件，也可以使用dom操作来控制视频的播放暂停，如play\(\)和pause\(\)方法。&lt;video&gt;&lt;/video&gt;之间插入浏览器不支持的&lt;video&gt;提示文本。同时video元素提供了width和height属性控制视频的尺寸，若设置了高度和宽度，所需视频空间在页面加载时保留，若没有设置，则浏览器在加载时不能保留特定的空间，页面会根据原始视频的大小改变。

&lt;video&gt;元素支持多个source元素，元素可以链接不同的视频文件，浏览器将使用第一个可识别的格式（.mp4，.webm和.ogg）。

## Canvas绘图

标签只是图形容器，必须使用脚本来绘制图形。

### Canvas - 图形

* 创建一个画布，画布在网页中是一个矩形框。默认没有边框和内容。

```
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000;"></canvas>
```

* 使用JavaScript绘制图像，Canvas元素本身没有绘图能力，所有绘制工作必须在JavaScript内部完成。

```js
var ctx=document.getElementById("myCanvas").getContext("2d");
ctx.fillStyle="#F00";
ctx.fillRect(0,0,150,75);
```

getContext\("2d"\)对象是内建的HTML5 对象，拥有多种绘制路径、矩形、圆形、字符等方法。

### Canvas - 文本

使用canvas绘制文本，重要的属性和方法：

* font：定义字体
* fillText\(text, x, y\)：在canvas上绘制实心的文本
* strokeText\(text, x, y\)：在canvas上绘制空心的文本

```js
var ctx=document.getElementById("myCanvas").getContext("2d");
ctx.font="30px Arial";
ctx.fillText("Hello World",10,50);
```



