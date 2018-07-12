# TCP/IP协议

TCP/IP协议将应用层、表示层、会话层合并为应用层，物理层和数据链路层合并为网络接口层。

![](/assets/TCP/IP.png)

## 三次握手

客户端和服务端在进行HTTP请求和返回的工程中，需要创建一个TCP connection（由客户端发起），HTTP不存在连接的概念，只有请求和响应。请求和响应都是数据包，它们之间的传输通道就是TCP connection。

![](/assets/三次握手.png)

第一次握手：主机A发送位码SYN=1，随机产生Seq number的数据包到服务器,主机B由SYN=1知道主机A要求建立联机

第二次握手：主机B收到请求后要确认联机信息，向主机A发送ACK number=（主机A的Seq + 1），SYN=1，随机产生Seq的数据包

第三次握手：主机A收到后检查ACK是否正确，若正确，主机A会再发送ACK number=（主机B的Seq + 1），主机B收到后确认seq值与ack=1则连接成功建立。

