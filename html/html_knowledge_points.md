# HTML 考点

- `Doctype`作用？标准模式与兼容模式各有什么区别？

```
1.<!DOCTYPE>声明位于文档首行，告知浏览器的解析器用什么文档标准进行解析。
2.DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
3.标准模式的排版和JS运作模式都是以浏览器支持的最高标准运行。
4.兼容模式中，页面以宽松的方式显示，模拟老式浏览器的行为以防止站点无法工作。
```

- `HTML5`为什么只需要写`<!DOCTYPE HTML>` ?

```
1.HTML5不基于SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为。
2.而HTML4.0基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。
```

- 行内元素有哪些？块级元素有哪些？空（`void`）元素有哪些？

```
行内元素：a、em、strong、span、i、img、b、label、select、textarea、sub、sup、q
块级元素：div、dl、dt、dd、ul、li、ol、p、h1-h6、table、fieldset-from控制组、from
空元素：br、hr
```

- `HTML5`有哪些新特性、移除了哪些元素？如何处理`HTML5`新标签的浏览器兼容问题？如何区别`HTML`和`HTML5`？

```
* HTML5 现在不是SGML的子集，主要是关于图像，位置，存储，多任务等功能的增加
    canvas绘画
    video/audio 媒介回放
    LocalStorage：本地离线存储，长期存储数据，浏览器关闭后数据不丢失
    sessionStorage：数据在浏览器关闭后自动删除
    语义化元素：article、footer、header、nav、section
    表单控件：calendar、date、time、email、url、search
    新的技术：WebWorker、WebSocket、Geolocation

* 移除的元素：
    纯表现的元素：big、center、font、s、strike、u
    对可用性产生负面影响的元素：frame、frameset、noframes

* 支持HTML5新标签
    IE6/7/8支持通过document.createElement方法产生的标签，利用这一特性让浏览器支持HTML5新标签
    浏览器支持新标签后，还需要添加标签默认的样式

* 区分HTML5：DOCTYPE声明、新增的结构元素、功能元素
```

- 简述一下对`HTML`语义化的理解？

```
1.去掉或者丢失样式的时候能够让页面呈现出清晰的结构
2.有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息，爬虫依赖于标签来确定上下文和各个关键字的权重
3.方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式渲染网页
4.便于团队开发和维护，语义化使得网页更具可读性，是进一步开发网页的必要步骤
```

- 浏览器是怎么对`HTML5`的离线存储资源进行管理和加载的？

```
在线的情况：浏览器发现HTML头部有manifest属性，它会请求manifest文件，如果第一次访问，浏览器会根据manifest文件的内容下载相应的资源
           并且进行离线存储。如果已访问过并且离线存储了，那么就使用离线的资源加载页面，然后浏览器对比新旧manifest文件，若没有改变
           ，就不做任何操作，如果改变，则重新下载文件并进行离线存储。
离线的情况：浏览器直接使用离线存储的资源。
```

- 描述一下`cookie`、`sessionStorage`和`localStorage`的区别？

```
cookie是网站为了标识用户身份而存储在用户本地终端的数据（通常经过加密）。
cookie数据始终在同源的http请求中携带（即使不需要），即会在浏览器和服务器来回传递。
sessionStorage和localStorage不会自动把数据发送给服务器，尽在本地保存。

存储大小：
    cookie数据大小不能超过4K。
    sessionStorage和localStorage虽然有存储大小限制，但比cookie大得多，可以达到5M或更大。

存储期限：
    localStorage    存储持久数据，浏览器关闭后数据不会丢失
    sessionStorage  数据在当前浏览器窗口关闭后自动删除
    cookie          设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。
```

- `iframe`的优点？`iframe`有哪些缺点？

```
优点：
1.解决加载缓慢的第三方内容，如图表和广告等的加载问题
2.并行加载脚本

缺点：
1.浏览器对同一域名的并发请求数是有限制的，一般为6个。iframe种的文档（即子文档）与父文档会共享连接，也就是说子文档和父文档的请求
  数回计算在一起，当并发请求数达到上限值时，子文档中的资源只能等待，直到前面的通信完成。
2.阻塞父窗口的onload事件。
3.脚本的执行是同步和阻塞的，将script元素放置在iframe之前，同样会阻塞iframe中资源的请求
4.制造点击劫持，将一个不可见的iframe或包含用户感兴趣内容的iframe覆盖在文档的某个位置上，诱使用户点击iframe中的内容。

通过js动态给iframe添加src属性，可以绕开这些问题
```

- 如何实现浏览器内多个标签页之间的通信？

```js
1.localStorage: localStorage在被添加、修改、删除时都会触发一个storage事件
**fitst.html**
<input id="name">
<input type="button" id="btn" value="提交">
<script type="text/javascript">
$(function(){
    $("#btn").click(function(){
        var name=$("#name").val();
        localStorage.setItem("name", name);
    });
});
</script>
**second.html**
<script type="text/javascript">
$(function(){
    window.addEventListener("storage", function(event){
        console.log(event.key + "=" + event.newValue);
    });
});
</script>

2.cookie + setInterval: 将传递的信息存储在cookie中，每隔一定事件读取cookie信息。
**first.html**
<input id="name">
<input type="button" id="btn" value="提交">
<script type="text/javascript">
$(function(){
    $("#btn").click(function(){
        var name=$("#name").val();
        document.cookie="name="+name;
    });
});
</script>
**second.html**
<script type="text/javascript">
$(function(){
    function getCookie(key) {
        return JSON.parse("{\"" + document.cookie.replace(/;\s+/gim,"\",\"").replace(/=/gim, "\":\"") + "\"}")[key];
    }
    setInterval(function(){
        console.log("name=" + getCookie("name"));
    }, 10000);
});
</script>
```

- 页面可见性（`Page Visibility API`）可以有哪些用途？

```
通过visibilityState的值检测页面当前是否可见，以及网页的打开时间等。
在页面被切换到其他后台进程时，自动暂停音乐或视频的播放。
```

- 实现不使用 border 画出 1px 高的线，在不同浏览器的标准模式与怪异模式下都能保持一致的效果。

```html
<div style="height:1px;overflow:hidden;background:red"></div>
```

- 网页验证码是干嘛的，是为了解决什么安全问题？

```
区分用户是计算机还是人的公共全自动程序。可以防止恶意破解密码、刷票、论坛灌水。
有效防止黑客对某一个特定注册用户用特定程序暴力破解方式进行不断的登录尝试。
```

- `title`与`h1`的区别、`b`与`strong`的区别、`i`与`em`的区别？

```
title属性没有明确意义，只是表示标题，h1则表示层次明确的标题，对页面信息的抓取也有很大影响。

strong是标明重点内容，有语气加强的含义，使用阅读设备阅读网络时：<strong>会重读，<b>是展示强调内容。

i内容展示为斜体，em表示强调的文本。
```

- meta 元素可以定义文档的哪些元数据？

```
meta元素可定义的元数据简要概括为4类：
1.声明HTML文档内容所用的字符编码
2.完善文档描述信息，让搜索引擎更容易解析索引，提升SEO
3.适配移动设备，使页面在各种尺寸的屏幕种显示正确
4.指定首选样式表、执行重载或重定向
```

- `meta viewport` 相关属性

```html
<!DOCTYPE html>
<!--H5标准声明，使用 HTML5 doctype，不区分大小写-->
<head lang="”en”">
  <!--标准的 lang 属性写法-->
  <meta charset="’utf-8′" />
  <!--声明文档使用的字符编码-->
  <meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>
  <!--优先使用 IE 最新版本和 Chrome-->
  <meta name="”description”" content="”不超过150个字符”" />
  <!--页面描述-->
  <meta name="”keywords”" content="””" />
  <!-- 页面关键词-->
  <meta name="”author”" content="”name," email@gmail.com” />
  <!--网页作者-->
  <meta name="”robots”" content="”index,follow”" />
  <!--搜索引擎抓取-->
  <meta name=”viewport” content=”initial-scale=1, maximum-scale=3,
  minimum-scale=1, user-scalable=no”>
  <!--为移动设备添加 viewport-->
  <meta name="”apple-mobile-web-app-title”" content="”标题”" />
  <!--iOS 设备 begin-->
  <meta name="”apple-mobile-web-app-capable”" content="”yes”" />
  <!--添加到主屏后的标题（iOS 6 新增）
是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏-->
  <meta name=”apple-itunes-app” content=”app-id=myAppStoreID,
  affiliate-data=myAffiliateData, app-argument=myURL”>
  <!--添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）-->
  <meta name="”apple-mobile-web-app-status-bar-style”" content="”black”" />
  <meta name=”format-detection” content=”telphone=no, email=no”/>
  <!--设置苹果工具栏颜色-->
  <meta name="”renderer”" content="”webkit”" />
  <!-- 启用360浏览器的极速模式(webkit)-->
  <meta http-equiv=”X-UA-Compatible” content=”IE=edge”>
  <!--避免IE使用兼容模式-->
  <meta http-equiv="”Cache-Control”" content="”no-siteapp”" />
  <!--不让百度转码-->
  <meta name="”HandheldFriendly”" content="”true”" />
  <!--针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓-->
  <meta name="”MobileOptimized”" content="”320″" />
  <!--微软的老式浏览器-->
  <meta name="”screen-orientation”" content="”portrait”" />
  <!--uc强制竖屏-->
  <meta name="”x5-orientation”" content="”portrait”" />
  <!--QQ强制竖屏-->
  <meta name="”full-screen”" content="”yes”" />
  <!--UC强制全屏-->
  <meta name="”x5-fullscreen”" content="”true”" />
  <!--QQ强制全屏-->
  <meta name="”browsermode”" content="”application”" />
  <!--UC应用模式-->
  <meta name="”x5-page-mode”" content="”app”" />
  <!-- QQ应用模式-->
  <meta name="”msapplication-tap-highlight”" content="”no”" />
  <!--windows phone 点击无高光
设置页面不缓存-->
  <meta http-equiv="”pragma”" content="”no-cache”" />
  <meta http-equiv="”cache-control”" content="”no-cache”" />
  <meta http-equiv="”expires”" content="”0″" />
</head>
```

- 简述一下 `src` 与 `href` 的区别

```
src 用于替换当前元素，href 用于在当前文档和引用资源之间确立联系。
src 是 source 的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求 src 资源时会将其指向的资源下载并应用到文档内，例如 js 脚本，img 图片和 frame 等元素
<script src ="js.js"></script> 当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将 js 脚本放在底部而不是头部

href 是 Hypertext Reference 的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加
<link href="common.css" rel="stylesheet"/> 那么浏览器会识别该文档为 css 文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用 link 方式来加载 css，而不是使用 @import 方式
```
