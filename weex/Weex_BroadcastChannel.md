# BroadcastChannel

&emsp;&emsp;不同的 `Weex` 页面使用的是不同的执行环境，即使全局变量也是互相隔离的，然而使用 `BroadcastChannel` 是可以实现跨页面通信的。

## `API`

&emsp;&emsp;`BroadcastChannel` 的构造函数只接受一个参数，那就是“频道名称”（`channel name`）。

```
const jb = new BroadcastChannel('007');
```

&emsp;&emsp;`BroadcastChannel` 接口的定义如下：

```
declare interface BroadcastChannel = {
  // 监听的频道名称，用来区分不同的频道（跨频道不可通信）
  name: string,
  // 用于在当前频道中广播消息
  postMessage: (message: any) => void;
  // 消息事件的处理函数。在频道中接收到广播消息之后，会给所有订阅者派发消息事件
  onmessage: (event: MessageEvent) => void;
  // 关闭当前频道
  close: () => void;
}
```

&emsp;&emsp;消息对象（`MessageEvent`）的类型定义如下：

```
declare interface MessageEvent = {
  type: string, // "message"
  data: any
}
```

## 注意事项

&emsp;&emsp;消息事件中的对象并没有深度复制。（这个特性可能会修改）

```
// 在页面 A 中：
const a = new BroadcastChannel('app')
const list = ['A', 'B']
a.postMessage({ list })

/ 在页面 B 中：
const b = new BroadcastChannel('app')
b.onmessage = function (event) {
  // the `event.data.list` is a reference of `list` in page A
  event.data.list.push('C')    // 将影响到页面A中的 list 对象。
}
```
