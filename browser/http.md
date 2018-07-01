# HTTP协议

## HTTP的主要特点：

* 简单快捷：每个资源的URI固定
* 灵活：通过一个HTTP，可以通过不同数据类型的传输
* 无连接：连接一次就会断开，不会保持连接
* 无状态：单从HTTP协议上，服务器不能区别两次连接者的身份

## HTTP报文的组成部分![](/assets/HTTP报文.png)HTTP方法：

* GET：获取资源

* POST：传输资源

* PUT：更新资源

* DELETE：删除资源

* HEAD：获得报文首部

## POST和GET的区别：

* GET在浏览器回退时是无害的，而POST会再次提交请求
* GET产生的URL地址可以被收藏，而POST不可以
* GET请求被会浏览器主动缓存，而POST不会，除非手动设置
* GET请求只能进行url编码，而POST支持多种编码方式
* GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留
* GET请求在URL中传送的参数是有长度限制的，而POST没有限制
* 对参数的数据类型，GET只接受ASCII字符，而POST没有限制
* GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息
* GET参数通过URL传递，POST放在Request body中 

## HTTP状态码

* 1XX：指示信息，表示请求已接受，继续处理
* 


