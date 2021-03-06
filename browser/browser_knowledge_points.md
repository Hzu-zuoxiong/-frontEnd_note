# 浏览器考点

- 简单说一下浏览器本地存储是怎样的

```
HTML5的Web Storage包括两种存储方式：sessionStorage和localStorage

sessionStorage:不是一种持久化的本地存储，仅仅是会话级别的存储。
localStorage：用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。
```

- 介绍下你对浏览器内核的理解

```
两部分：渲染引擎（layout）和JS引擎
渲染引擎：负责取得网页的内容（HTML、XML、图像等）、整理讯息（加入CSS等），以及计算网页的显示方式，然后输出到显示器或打印机。
JS引擎：解析和执行JavaScript来实现网页的动态效果。
最开始渲染引擎和JS引擎没有明确的区分，后来JS引擎越来越独立，内核倾向于指渲染引擎。
```

- 常见的浏览器内核有哪些？

```
Trident内核：IE
Gecko内核：Firefox
Presto内核：Opera（Opera内核原为Presto，现为Blink）
Webkit内核：Safari、Chrome
```

- 解释以下 JavaScript 的同源策略

```
概念：同源策略是客户端加本的重要安全度量标准。其目的是防止某个文档或脚本从多个不同源装载。
这里的同源策略指的是：协议、域名、端口相同，同源策略是一种安全协议，指一段脚本只能读取来之同一来源的窗口和文档的属性。

为什么要有同源限制：
举例说明：比如一个黑客程序，利用iframe把真正的银行登录页面嵌到他的页面上，当你使用真实的用户名，密码登录时，他的页面就可以通过
         JS读取到你表单中input的内容，这样用户名，密码就轻松到手了。
```
