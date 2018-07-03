# 定时器工作机制

JS有两个定时器，setTimeout 和 setInterval。

## setTimeout

**定义：**在指定的延时时间之后调用一个函数或者代码片段。

**语法：**第一个参数为回调函数，第二个参数为延时时间，setTimeout方法返回一个数字，为该定时器的ID，这个ID是定时器的唯一标识，用clearTimeout可以消除定时器。

## setInterval

**定义：**周期性地调用一个函数或者代码片段。

**语法：**与 setTimeout 一样。

## 第二个参数（delay）

由于javascript的事件循环机制，导致第二个参数并不代表延迟delay毫秒之后立即执行回调函数，而是将回调函数加入事件队列。

* setTimeout：延时delay毫秒之后，将回调函数加入事件队列。
* setInterval：延时delay毫秒之后，判断事件队列中是否存在未执行的回调函数，若没有，则将回调函数加入事件队列；若有则不往事件队列里加入回调函数。

## 代码演示：

### setTimout:

```js
var timerStart1 = new Date();
setTimeout(function () {
  console.log('第一个setTimeout回调执行等待时间：', new Date() - timerStart1);

  var timerStart2 = new Date();
  setTimeout(function () {
    console.log('第二个setTimeout回调执行等待时间：', new Date() - timerStart2);
  }, 100);
}, 100);
// 第一个setTimeout回调执行等待时间： 102
// 第二个setTimeout回调执行等待时间： 100
```

结果与预想中一样，每个定时器都在100ms之后触发。但是一旦在代码中加入耗时的任务，结果就跟期望的不一样。

```js
var timerStart1 = new Date();
setTimeout(function () {
  console.log('第一个setTimeout回调执行等待时间：', new Date() - timerStart1);

  var timerStart2 = new Date();
  setTimeout(function () {
    console.log('第二个setTimeout回调执行等待时间：', new Date() - timerStart2);
  }, 100);

  heavyTask();  // 耗时任务
}, 100);

var loopStart = new Date();
heavyTask(); // 耗时任务
console.log('heavyTask耗费时间：', new Date() - loopStart);

function heavyTask() {
  var s = new Date();
  while(new Date() - s < 1000) {
  }
}
// heavyTask耗费时间： 1000
// 第一个setTimeout回调执行等待时间： 1010
// 第二个setTimeout回调执行等待时间： 1025
```

从结果可以看出，两个setTimeout的等待事件不再是100ms了。

1. 首先，第一个耗时任务（`heavyTask()`）开始执行，大约`1000ms`才能执行完毕。
2. 耗时任务执行时，过了`100ms`， 第一个`setTimeout`的回调函数期望执行，于是被加入到事件队列，但是耗时任务还没执行完，所以只能在队列中等待，直到耗时任务执行完毕它才开始执行，所以结果中我们开的看到的是：`第一个setTimeout回调执行等待时间： 1010`
3. 第一个`setTimeout`回调执行时，又开启了第二个`setTimeout`。 但第一个`setTimeout`存在一个耗时任务，所以第二个setTimeout等待了 1000ms 才开始执行。

![](/assets/setTimeout.png)

### setInterval：

```js
var intervalStart = new Date();
setInterval(function () {
  console.log('interval距定义定时器的时间：', new Date() - loopStart);
}, 100);

var loopStart = new Date();
heavyTask();
console.log('heavyTask耗费时间：', new Date() - loopStart);

function heavyTask() {
  var s = new Date();
  while(new Date() - s < 1000) {
  }
}
// heavyTask耗费时间： 1001
// interval距定义定时器的时间： 1014
// interval距定义定时器的时间： 1101
// interval距定义定时器的时间： 1201
```

上面这段代码，我们期望每隔`100ms`就打出一条日志。相对于`setTimeout`的区别，`setInterval`在**准备把回调函数加入到事件队列的时候，会判断队列中是否还有未执行的回调**，如果有的话，它就不会再往队列中添加回调函数。 不然，会出现多个回调同时执行的情况。

![](/assets/setInterval.png)

