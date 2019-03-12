# 跨域问题

## 同源策略：

同源策略（Same origin policy）是一种约定，是浏览器最基本的安全功能，若缺少同源策略，很容易受到 XSS、CSFR 等攻击。所谓同源指“协议 + 域名 + 端口”三者相同，即便两个不同的域名指向同一个 IP 地址，也非同源。

同源策略限制以下行为：

1. Cookie、LocalStorage 和 IndexDB 无法获取
2. DOM 和 JS 对象无法获得
3. AJAX 请求不能发送

## 跨域解决方案

1. 通过 JSONP 跨域
2. document.domain + iframe 跨域
3. location.hash + iframe
4. window.name + iframe 跨域
5. postMessage 跨域
6. 跨域资源共享（CORS）
7. Nginx 代理跨域
8. node.js 中间件代理跨域
9. WebSocket 协议跨域

### 通过 JSONP 跨域

在 HTML 标签里，如 script、img 等标签没有跨域限制，可以利用此特点动态创建 script 标签，再请求一个带参网址实现跨域通信。

JSONP 缺点：只能实现 GET 一种请求。

- 原生实现

```js
<script>
    var script = document.creatElement('script');
    script.type = 'text/javascript';
    script.src = 'http://www.domain2.com:8080/login?user=admin&callback=onBack';
    document.head.appendChild(script);

    function onBack(res) {
        alert(JSON.stringify(res));
    }
</script>
```

- Jquery Ajax

```js
$.ajax({
  url: 'http://www.domain2.com:8080/login',
  type: 'get',
  dataType: 'jsonp', // 请求方式为jsonp
  jsonpCallback: 'onBack', // 自定义回调函数名
  data: {}
});
```

### 通过 document.domain + iframe 跨域

此方案仅限主域相同，子域不同的跨域场景。

实现原理：两个页面通过 JS 强制设置 document.domain 为基础主域，实现同域。

1.）父窗口：\([http://www.domain.com/a.html\)](http://www.domain.com/a.html%29)

```js
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>
```

2.）子窗口：\([http://child.domain.com/b.html\)](http://child.domain.com/b.html%29)

```js
<script>
  document.domain = 'domain.com'; // 获取父窗口中变量 alert('get js data from
  parent ---> ' + window.parent.user);
</script>
```

### 通过 location.hash + iframe 跨域

实现原理：a 页面通过中间页 c 与页面 c 相互通信。三个页面，不同域之间利用 iframe 的 location.hash 传值，相同域之间用 js 访问通信。

1.）a.html：\([http://www.domain1.com/a.html](http://www.domain1.com/a.html%29) \)

```js
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 向b.html传hash值
    setTimeout(function() {
        iframe.src = iframe.src + '#user=admin';
    }, 1000);

    // 开放给同域c.html的回调方法
    function onCallback(res) {
        alert('data from c.html ---> ' + res);
    }
</script>
```

2.）b.html：\([http://www.domain2.com/b.html ](http://www.domain2.com/b.html%29)\)

```js
<iframe id="iframe" src="http://www.domain1.com/c.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 监听a.html传来的hash值，再传给c.html
    window.onhashchange = function () {
        iframe.src = iframe.src + location.hash;
    };
</script>
```

3.）c.html：\([http://www.domain1.com/c.html](http://www.domain1.com/c.html%29) \)

```js
<script>
    // 监听b.html传来的hash值
    window.onhashchange = function () {
        // 再通过操作同域a.html的js回调，将结果传回
        window.parent.parent.onCallback('hello: ' + location.hash.replace('#user=', ''));
    };
</script>
```

### 通过 postMessage 跨域

postMessage 是 HTML5 XMLHttpRequest Level2 的 API，用于解决以下问题：

1. 页面和其打开的新窗口的数据传输
2. 多窗口之间消息传递
3. 页面与嵌套的 iframe 消息传递
4. 上面三个场景的跨域数据传递

1.）a.html：\([http://www.domain1.com/a.html](http://www.domain1.com/a.html%29) \)

```js
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');
    iframe.onload = function() {
        var data = {
            name: 'aym'
        };
        // 向domain2传送跨域数据
        iframe.contentWindow.postMessage(JSON.stringify(data), 'http://www.domain2.com');
    };

    // 接受domain2返回数据
    window.addEventListener('message', function(e) {
        alert('data from domain2 ---> ' + e.data);
    }, false);
</script>
```

2.）b.html：\([http://www.domain2.com/b.html](http://www.domain2.com/b.html%29) \)

```js
<script>
    // 接收domain1的数据
    window.addEventListener('message', function(e) {
        alert('data from domain1 ---> ' + e.data);

        var data = JSON.parse(e.data);
        if (data) {
            data.number = 16;

            // 处理后再发回domain1
            window.parent.postMessage(JSON.stringify(data), 'http://www.domain1.com');
        }
    }, false);
</script>
```

### 跨域资源共享（CORS）

CORS 是一个 W3C 标准，全称是“跨域资源共享”（Cross-origin resource sharing），它允许浏览器向跨域源服务器发出 XMLHttpRequest 请求。CORS 需要浏览器和服务器同时支持。整个 CORS 通信过程，都是浏览器自动完成，不需要用户参与。

**前端设置**

- 原生 Ajax

```js
var xhr = new XMLHttpRequest(); // IE8/9需用window.XDomainRequest兼容

// 前端设置是否带cookie
xhr.withCredentials = true;

xhr.open('post', 'http://www.domain2.com:8080/login', true);
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('user=admin');

xhr.onreadystatechange = function() {
  if (xhr.readyState == 4 && xhr.status == 200) {
    alert(xhr.responseText);
  }
};
```

- Jquery Ajax

```js
$.ajax({
    ...
   xhrFields: {
       withCredentials: true    // 前端设置是否带cookie
   },
   crossDomain: true,   // 会让请求头中包含跨域的额外信息，但不会含cookie
    ...
});
```

**后端设置**

```js
var http = require('http');
var server = http.createServer();
var qs = require('querystring');

server.on('request', function(req, res) {
  var postData = '';

  // 数据块接收中
  req.addListener('data', function(chunk) {
    postData += chunk;
  });

  // 数据接收完毕
  req.addListener('end', function() {
    postData = qs.parse(postData);

    // 跨域后台设置
    res.writeHead(200, {
      'Access-Control-Allow-Credentials': 'true', // 后端允许发送Cookie
      'Access-Control-Allow-Origin': 'http://www.domain1.com', // 允许访问的域（协议+域名+端口）
      /*
       * 此处设置的cookie还是domain2的而非domain1，因为后端也不能跨域写cookie(nginx反向代理可以实现)，
       * 但只要domain2中写入一次cookie认证，后面的跨域接口都能从domain2中获取cookie，从而实现所有的接口都能跨域访问
       */
      'Set-Cookie': 'l=a123456;Path=/;Domain=www.domain2.com;HttpOnly' // HttpOnly的作用是让js无法读取cookie
    });

    res.write(JSON.stringify(postData));
    res.end();
  });
});

server.listen('8080');
console.log('Server is running at port 8080...');
```

### 通过 WebSocket 协议跨域

WebSocket 最大的特点是服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话。

其他特点：

1. 建立再 TCP 协议之上，服务端的实现比较容易。
2. 与 HTTP 协议有良好的兼容性，默认端口也是 80 和 443，并且握手阶段采用 HTTP 协议。
3. 数据格式比较轻量，性能开销小，通信高效。
4. 可以发送文本，也可以发送二进制数据。
5. 偶同源限制，客户端可以与任意服务器通信。
6. 协议标识符是 WS（如果加密，则为 WSS），服务器网址就是 URL。

前端代码：

```js
<div>user input：<input type="text"></div>
<script src="./socket.io.js"></script>
<script>
var socket = io('http://www.domain2.com:8080');

// 连接成功处理
socket.on('connect', function() {
    // 监听服务端消息
    socket.on('message', function(msg) {
        console.log('data from server: ---> ' + msg);
    });

    // 监听服务端关闭
    socket.on('disconnect', function() {
        console.log('Server socket has closed.');
    });
});

document.getElementsByTagName('input')[0].onblur = function() {
    socket.send(this.value);
};
</script>
```

后端代码：

```js
var http = require('http');
var socket = require('socket.io');

// 启http服务
var server = http.createServer(function(req, res) {
  res.writeHead(200, {
    'Content-type': 'text/html'
  });
  res.end();
});

server.listen('8080');
console.log('Server is running at port 8080...');

// 监听socket连接
socket.listen(server).on('connection', function(client) {
  // 接收信息
  client.on('message', function(msg) {
    client.send('hello：' + msg);
    console.log('data from client: ---> ' + msg);
  });

  // 断开处理
  client.on('disconnect', function() {
    console.log('Client socket has closed.');
  });
});
```

参考链接：[https://segmentfault.com/a/1190000011145364](https://segmentfault.com/a/1190000011145364)
