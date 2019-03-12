# Event Loop

## 单线程

JavaScript 语言的一大特点就是单线程。作为浏览器脚本语言，JavaScript 的主要用途是与用户互动，以及操作 DOM，这决定了它只能是单线程。HTML5 提出了 Web Worker 标准，允许 JavaScript 脚本创建多个线程，但子线程完全受主线程控制，且不得操作 DOM。

## 任务队列

单线程意味着所有任务需要排队，前一个任务结束，才会执行后一个任务。任务可以分为两种，一种是`同步任务`，另一种是`异步任务`。同步任务指在主线程上排队执行的任务，只有前一个任务完成，才能执行后一个任务。异步任务指不进入主线程，而进入“任务队列”\(task queue\)的任务，只有“任务队列”通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

异步允许的机制如下:

1. 所有同步任务都在主线程上执行，形成一个[执行栈](http://www.ruanyifeng.com/blog/2013/11/stack.html)（execution context stack）。
2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
4. 主线程不断重复上面的第三步。

只要主线程空了，就会去读取“任务队列”。这就是 JavaScript 的运行机制。

## 事件和回调函数

“任务队列”中的事件，除了 IO 设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等）。只要指定过回调函数，这些事件发生时就会进入“任务队列”，等待主线程读取。

“回调函数”，就是会被主线程挂起来的代码。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。

“任务队列”是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，“任务队列”的第一个事件会就自动进入主线程。

## Event Loop（事件循环）

主线程从“任务队列”中读取事件，这个过程是循环的，所以整个运行机制称为 Event Loop。

主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种 API，它们在“任务队列”中加入各种事件。只要栈中的代码执行完毕，主线程就会去读取“任务队列”，依次执行其对应的回调函数。

## Node.js 的 Event Loop

Node.js 的运行机制如下：

1. V8 引擎解析 JavaScript 脚本。
2. 解析后的代码，调用 Node API。
3. [libuv 库](https://github.com/libuv/libuv)负责 Node API 的执行。它将不同的任务分配给不同的线程，形成一个 Event Loop（事件循环），以异步的方式将任务的执行结果返回给 V8 引擎。
4. V8 引擎再将结果返回给用户。

除了 setTimeout 和 setInterval 两个方法，Node.js 还提供了另外两个与“任务队列”有关的方法：process.nextTick 和 setImmediate。

process.nextTick 方法可以在当前“执行栈”的尾部----下一次 Event Loop（主线程读取“任务队列”）之前触发回调函数。也就是说它指定的任务总是发生在所有异步任务之前。setImmediate 方法则在当前“任务队列”的尾部添加事件，它指定的任务总是在下一次 Event Loop 时执行，与 setTimeout\(fn, 0\)很像。

```js
setImmediate(function() {
  setImmediate(function A() {
    console.log(1);
    setImmediate(function B() {
      console.log(2);
    });
  });

  setTimeout(function timeout() {
    console.log('TIMEOUT FIRED');
  }, 0);
});
// 1
// TIMEOUT FIRED
// 2
```
