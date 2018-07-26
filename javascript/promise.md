# Promise

Promise是异步编程的一种解决方法，比传统的回调函数更合理更强大。

## 特点

* 对象的状态不受外界影响，有三种状态：Pengding\(进行中\)、Fulfilled\(已成功\)、Rejected\(已失败\)。
* 一旦状态改变就不会再变。Promise对象的状态改变只有两种可能：从Pengding变为Fulfilled；从Pending变为Rejected。

Promise对象可以将异步操作以同不操作的流程表达出来，避免了层层嵌套的回掉函数。

Promise也有缺点：

* 无法取消Promise，**一旦新建它就会立即执行**，无法中途取消。
* Promise内部抛出的错误，不会反应到外部。
* 当处于Pengding状态，无法得知目前进展到哪一个阶段（刚开始还是即将结束）。

## 用法

Promise构造函数接收一个函数作为参数，该函数的两个参数分别是resolve和reject，它们是两个函数，由JS引擎提供。

Promise实例生成之后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

```js
// Promise新建后会立即执行
var promise = new Promise(function (resolve, reject) {
    console.log('Promise');
    if(// 异步操作成功) {
        resolve('resolve');
    } else {
        reject('error');
    }
});

// 指定resolved状态和rejected状态的回调函数
promise.then(function (value) {
    console.log(value);
}, function(error) {
    console.log(error);
});

console.log('Hi');

// Promise
// Hi
// resolve
```



