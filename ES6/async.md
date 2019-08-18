# async / await 浅析与应用

## 什么是 async / await ?

ES7 引入 async 函数，使得 JavaScript 对异步操作更加方便，也使得异步代码更容易阅读。

```javascript
// Generator 函数
var gen = function*() {
  var f1 = yield readFile("/etc/fstab");
  var f2 = yield readFile("/etc/shells");
  console.log(f1.toString());
  console.log(f2.toString());
};
// async 函数
var asyncReadFile = async function() {
  var f1 = await readFile("/etc/fstab");
  var f2 = await readFile("/etc/shells");
  console.log(f1.toString());
  console.log(f2.toString());
};
```

总的来说，`async` 函数是 `Generator` 函数的语法糖，将 `Generator` 函数的星号（\*）替换成 `async`，将 `yield` 替换成 `await`。

相对于 `Generator` 函数，`async` 函数有以下改进：

- 内置执行函数。`Generator` 函数返回的是 `Iterator` 对象，函数的执行需要调用 `next` 方法。
  更好的语义化。`async` 和 `await`，对比星号和 `yield`，语义更加清晰。
- 返回值是 `Promise` 对象。相比于 `Generator` 函数返回 `Iterator` 对象，使用起来更加方便，可以使用 `then` 方法指定下一步操作。

## 语法

- `async` 函数的语法相对简单，主要难度在错误处理。

`async` 函数返回一个 `Promise` 对象。`async` 函数如果内部抛出错误，会导致返回的 `Promise` 对象变为 `reject` 状态。如果 `await` 后面的异步操作出错，相当于整个 `async` 函数被 `reject`，可以使用 `try catch` 对其进行捕获。

```javascript
async function f() {
  await Promise.reject("出错了");
  await Promise.resolve("hello world"); // 不会执行
}

// try catch 捕获错误
async function f() {
  try {
    await Promise.reject("出错了");
  } catch (e) {
    console.log(e);
  }
  return await Promise.resolve("hello world");
}
```

`async` 函数返回的 `Promise` 对象，必须等到内部所有 `await` 命令的 `Promise` 对象执行完，才会发生状态改变。

```javascript
async function getTitle(url) {
  let response = await fetch(url);
  let html = await response.text();
  return html.match(/<title>([\s\S]+)<\/title>/i)[1];
}
getTitle(
  "http://wiki.qipeipu.net/pages/editblogpost.action?pageId=21569739"
).then(console.log);
// 编辑 - async / await 浅析与应用 - 吴作雄 - 巴图鲁wiki
```

`await` 命令后面是一个 `Promise` 对象，如果不是会被转成一个立即 `resolve` 的 `Promise` 对象。

## async 应用

```javascript
//请写出输出内容
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}
async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(function() {
  console.log("setTimeout");
}, 0);

async1();

new Promise(function(resolve) {
  console.log("promise1");
  resolve();
}).then(function() {
  console.log("promise2");
});
console.log("script end");

/*输出：
script start
async1 start
async2
promise1
script end
async1 end
promise2
setTimeout
*/
```

### 知识点：宏任务(macrotask)、微任务(microtask)、运行机制

**宏任务**：可以理解为每次执行栈执行的代码就是一个宏任务，主要包含：`script`(整体代码)、`setTimeout`、`setInterval` 等。

**微任务**：可以理解为当前 `macrotask` 执行结束后立即执行的任务，主要包含：`Promise.then`、`process.nextTick` 等。

**运行机制**：

1. 执行一个宏任务（栈中没有就从事件队列中获取）
2. 执行时遇到微任务，将其加入微任务队列
3. 宏任务执行完毕，执行微任务队列中的微任务
4. 微任务队列执行完成之后，开始检查渲染，GUI 线程接管渲染
5. 渲染完毕之后，JS 线程开始执行下一个宏任务

### await 的作用：

从字面上的意思 `await` 就是等待，很多人会以为 `await` 会等后面的表达式执行完之后继续执行后面的代码，实际上 `await` 是一个让出线程的标志，会先执行 `await` 后面的表达式，将 `await` 后面的代码加入到微任务队列中，然后跳出整个 `async` 函数继续执行代码。

```javascript
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

// 等价于
async function async1() {
  console.log("async1 start");
  Promise.resolve(async2()).then(() => {
    console.log("async1 end");
  });
}
```

## 参考链接：

http://es6.ruanyifeng.com/#docs/async

https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/7

https://v8.js.cn/blog/fast-async/
