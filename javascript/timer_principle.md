# 定时器

&emsp;&emsp;“任务队列”可以放置定时事件，即指定某些代码在多少时间之后执行，这叫做“定时器”功能。定时器功能主要由 `setTimeout()` 和 `setInterval()` 这个两个函数完成。

> 注意：`setTimeout` 的第一个参数必须是**需要编译的代码**或者是**一个函数方法**，而如果直接传入一行可执行代码，那么会立即执行，没有延迟效果。

## `setTimeout`

**定义：** 在指定的延时时间之后调用一个函数或者代码片段。

**语法：**

```
/**
* 参数：
* function: delay 毫秒之后执行的函数
* delay: 延迟的毫秒数，如果省略该参数，delay 默认取值 0。实际的延迟时间可能会比 delay 值长
* param1, ..., paramN: 附加参数，作为参数传递给 function 或执行字符串
*/
var timeoutID = setTimeout(function[, delay]);
var timeoutID = setTimeout(function[, delay, param1, param2, ...]);
// 可以使用字符串代替 function，在 delay 毫秒后执行字符串（不推荐）
var timeoutID = setTimeout(code[, delay]);
```

> 注：IE9 及更早的 IE 浏览器不支持第二种语法中向延迟函数传递额外参数的功能。

`setTimeout(fn,0)` 的含义是，它在"任务队列"的尾部添加一个事件，因此要等到同步任务和"任务队列"现有的事件都处理完，才会得到执行。

`HTML5` 标准规定了 `setTimeout()` 的第二个参数的最小值，不得低于 4 毫秒，如果低于这个值，就会自动增加。另外，对于 `DOM` 的变动（尤其是涉及页面重新渲染的部分），通常不会立即执行，而是每 `16` 毫秒执行依次，这时使用 `requestAnimationFrame()` 的效果好于 `setTimeout()`。

`setTimeout()` 只是将事件插入“任务队列”，必须等到当前代码执行完，主线程才会执行它指定的回调函数。若当前代码耗时长，则无法保证回调函数一定会在 `setTimeout()` 指定的时间执行。

## `setInterval`

**定义：** 周期性地调用一个函数或者代码片段。

**语法：** 与 `setTimeout` 一样。

## 第二个参数（delay）

&emsp;&emsp;由于 `javascript` 的事件循环机制，导致第二个参数并不代表延迟 `delay` 毫秒之后立即执行回调函数，而是将回调函数加入事件队列。

- `setTimeout`：延时 `delay` 毫秒之后，将回调函数加入事件队列。
- `setInterval`：延时 `delay` 毫秒之后，判断事件队列中是否存在未执行的回调函数，若没有，则将回调函数加入事件队列；若有则不往事件队列里加入回调函数。

## 代码演示：

### `setTimout`:

```js
var timerStart1 = new Date();
setTimeout(function() {
  console.log('第一个setTimeout回调执行等待时间：', new Date() - timerStart1);

  var timerStart2 = new Date();
  setTimeout(function() {
    console.log('第二个setTimeout回调执行等待时间：', new Date() - timerStart2);
  }, 100);
}, 100);
// 第一个setTimeout回调执行等待时间： 102
// 第二个setTimeout回调执行等待时间： 100
```

结果与预想中一样，每个定时器都在 `100ms` 之后触发。但是一旦在代码中加入耗时的任务，结果就跟期望的不一样。

```js
var timerStart1 = new Date();
setTimeout(function() {
  console.log('第一个setTimeout回调执行等待时间：', new Date() - timerStart1);

  var timerStart2 = new Date();
  setTimeout(function() {
    console.log('第二个setTimeout回调执行等待时间：', new Date() - timerStart2);
  }, 100);

  heavyTask(); // 耗时任务
}, 100);

var loopStart = new Date();
heavyTask(); // 耗时任务
console.log('heavyTask耗费时间：', new Date() - loopStart);

function heavyTask() {
  var s = new Date();
  while (new Date() - s < 1000) {}
}
// heavyTask耗费时间： 1000
// 第一个setTimeout回调执行等待时间： 1010
// 第二个setTimeout回调执行等待时间： 1025
```

从结果可以看出，两个 `setTimeout` 的等待事件不再是 `100ms` 了。

1. 首先，第一个耗时任务（`heavyTask()`）开始执行，大约 `1000ms` 才能执行完毕。
2. 耗时任务执行时，过了 `100ms`， 第一个 `setTimeout` 的回调函数期望执行，于是被加入到事件队列，但是耗时任务还没执行完，所以只能在队列中等待，直到耗时任务执行完毕它才开始执行，所以结果中我们开的看到的是：第一个 `setTimeout` 回调执行等待时间： `1010`
3. 第一个 `setTimeout` 回调执行时，又开启了第二个 `setTimeout`。 但第一个 `setTimeout` 存在一个耗时任务，所以第二个 `setTimeout` 等待了 `1000ms` 才开始执行。

![](/assets/javascript/setTimeout.png)

### setInterval：

```js
var intervalStart = new Date();
setInterval(function() {
  console.log('interval距定义定时器的时间：', new Date() - loopStart);
}, 100);

var loopStart = new Date();
heavyTask();
console.log('heavyTask耗费时间：', new Date() - loopStart);

function heavyTask() {
  var s = new Date();
  while (new Date() - s < 1000) {}
}
// heavyTask耗费时间： 1001
// interval距定义定时器的时间： 1014
// interval距定义定时器的时间： 1101
// interval距定义定时器的时间： 1201
```

上面这段代码，我们期望每隔 `100ms` 就打出一条日志。相对于 `setTimeout` 的区别， `setInterval` 在**准备把回调函数加入到事件队列的时候，会判断队列中是否还有未执行的回调**，如果有的话，它就不会再往队列中添加回调函数。 不然，会出现多个回调同时执行的情况。

![](/assets/javascript/setInterval.png)

参考链接：[https://segmentfault.com/a/1190000003764106](https://segmentfault.com/a/1190000003764106)
