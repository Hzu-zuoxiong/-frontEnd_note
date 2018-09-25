# HTML考点

* `Doctype`作用？标准模式与兼容模式各有什么区别？

```
1.<!DOCTYPE>声明于HTML文档中的第一行。告知浏览器的解析器用什么文档标准进行解析。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
2.标准模式的排版和JS运作模式都是以浏览器支持的最高标准运行。兼容模式中，页面以宽松的方式显示，模拟老式浏览器的行为以防止站点无法工作。
```

* `HTML5`为什么只需要写`<!DOCTYPE HTML>` ?

```
1.HTML5不基于SGML，因此不需要对DTD进行引用，但是需要doctype来规范浏览器的行为。
2.而HTML4.0基于SGML，所以需要对DTD进行引用，才能告知浏览器文档所使用的文档类型。
```

* 行内元素有哪些？块级元素有哪些？空（`void`）元素有哪些？

```
行内元素：a、em、strong、span、i、img、b、label、select、textarea、sub、sup、q
块级元素：div、dl、dt、dd、ul、li、ol、p、h1-h6、table、fieldset-from控制组、from
空元素：br、hr
```

* `HTML5`有哪些新特性、移除了哪些元素？如何处理`HTML5`新标签的浏览器兼容问题？如何区别`HTML`和`HTML5`？

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

* 简述一下对`HTML`语义化的理解？

```
用正确的标签做正确的事情。
HTML语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析。
即使在没有样式CSS情况下，也以一种文档格式显示，并且容易阅读。
搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO。
使阅读源代码的人对网站更容易将网站分开，便于阅读理解和维护。
```

* 浏览器是怎么对`HTML5`的离线存储资源进行管理和加载的？

```
在线的情况：浏览器发现HTML头部有manifest属性，它会请求manifest文件，如果第一次访问，浏览器会根据manifest文件的内容下载相应的资源
           并且进行离线存储。如果已访问过并且离线存储了，那么就使用离线的资源加载页面，然后浏览器对比新旧manifest文件，若没有改变
           ，就不做任何操作，如果改变，则重新下载文件并进行离线存储。
离线的情况：浏览器直接使用离线存储的资源。
```

* 描述一下`cookie`、sessionStorage和localStorage的区别？

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



