# Promise

Promise 是异步编程的一种解决方法，比传统的回调函数更合理更强大。

## 特点

- 对象的状态不受外界影响，有三种状态：Pengding\(进行中\)、Fulfilled\(已成功\)、Rejected\(已失败\)。
- 一旦状态改变就不会再变。Promise 对象的状态改变只有两种可能：从 Pengding 变为 Fulfilled；从 Pending 变为 Rejected。

Promise 对象可以将异步操作以同不操作的流程表达出来，避免了层层嵌套的回掉函数。

Promise 也有缺点：

- 无法取消 Promise，**一旦新建它就会立即执行**，无法中途取消。
- Promise 内部抛出的错误，不会反应到外部。
- 当处于 Pengding 状态，无法得知目前进展到哪一个阶段（刚开始还是即将结束）。

## 用法

### 基本用法

Promise 构造函数接收一个函数作为参数，该函数的两个参数分别是 resolve 和 reject，它们是两个函数，由 JS 引擎提供。

Promise 实例生成之后，可以用 then 方法分别指定 resolved 状态和 rejected 状态的回调函数。如果 Promise 对象状态变为 resolved，则会调用 then 方法指定的回调函数，then 方法返回一个新的 Promise 实例，因此可以采用链式写法。若使用 then 方法依次指定两个回调函数，则**第一个回调函数完成之后，会将返回结果作为参数传入第二个回调函数**。

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

// 输出结果
// Promise
// Hi
// resolve
```

### Promise.prototype.catch\(\)

Promise.prototype.catch 方法是 .then\(null, rejection\)的别名，用于指定发生错误时的回调函数。如果 Promise 对象异步操作抛出错误，状态就会变为 rejected，则会调用 catch 方法指定的回调函数。then 方法指定的回调函数，如果运行中抛出错误，也会被 catch 方法捕获。

**注：如果 Promise 状态已经变成 resolved，再抛出错误是无效的；Promise 对象的错误具有“冒泡“性质，会一直向后传递，直到被捕获为止；Promise 内部的错误不会影响到 Promise 外部的代码。**

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  // 无效， Promise状态一旦改变，则永久保持该状态
  throw new Error('test');
});
promise
  .then(function(value) {
    console.log(value);
  })
  .catch(function(error) {
    console.log(error);
  });
// ok

const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});

setTimeout(() => {
  console.log(123);
}, 2000);
// Uncaught (in promise) ReferenceError: x is not defined
// 123
```

### Promise.prototype.finally\(\)

Promise.prototype.finally\(\)方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。即不管 Promise 最后的状态，在执行完 then 或 catch 指定的回调函数之后，都会执行 finally 方法指定的回调函数。

finally 方法的回调函数不接受任何参数，意味着没有办法知道前面的 Promise 状态。则 finally 里面的操作应该是与状态无关的，不依赖于 Primise 的执行结果。

```js
promise.then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
```

参考链接：[http://es6.ruanyifeng.com/\#docs/promise](http://es6.ruanyifeng.com/#docs/promise)
