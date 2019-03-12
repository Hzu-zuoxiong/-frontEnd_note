# async 函数

ES2017 标准引入了 async 函数，使得异步操作变得更加方便，它是 Generator 函数的语法糖。

async 函数对 Generator 函数的改进：

1. 内置执行器：Generator 函数的执行必须靠执行器，所以才有 co 模块，而 async 函数自带执行器。
2. 语义更清晰：async 表示函数里有异步操作，await 表示紧跟在后面的表达式需要等待结果。
3. 更好的适用性：async 函数的 await 命令后面，可以是 Promise 对象和原始类型的值。
4. 返回值是 Promise：Generator 函数的返回值是 Iterator 对象，async 函数的返回值是 Promise 对象。

async 函数可以看作多个异步操作，包装成一个 Promise 对象，而 await 命令就是内部 then 命令的语法糖。

async 函数的实现原理，就是将 Generator 函数和自动执行器包装再一个函数里。

```js
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function*() {
    // ...
  });
}

function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch (e) {
        return reject(e);
      }
      if (next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(
        function(v) {
          step(function() {
            return gen.next(v);
          });
        },
        function(e) {
          step(function() {
            return gen.throw(e);
          });
        }
      );
    }
    step(function() {
      return gen.next(undefined);
    });
  });
}
```

## 基本用法

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```js
function timeout(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
```

## 语法

### 返回 Promise 对象

async 函数返回一个 Promise 对象，内部 return 语句返回的值，会成为 then 方法回调函数的参数。async 函数内部抛出错误，会导致 Promise 对象变为 reject 状态。

### Promise 对象的状态变化

async 函数必须等到内部所有的 await 命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到 return 语句或抛出错误。

### await 命令

- await 命令后面是一个 Promise 对象。如果不是，会被转成一个立即 resolve 的 Promise 对象。
- await 命令只能用在 async 函数之中，如果用在普通函数，就会报错。
- 只要一个 await 语句后面的 Promise 变为 reject，那么整个 async 函数都会中断执行。（可以使用 try...catch 或再跟一个 catch 方法变通）

```js
// 使用try...catch实现多次重复尝试
const superagent = require('superagent');
const NUM_RETRIES = 3;

async function test() {
  let i;
  for (i = 0; i < NUM_RETRIES; ++i) {
    try {
      await superagent.get('http://google.com/this-throws-an-error');
      break;
    } catch (err) {}
  }
  console.log(i); // 3
}

test();

// 使用catch
async function f() {
  await Promise.reject('出错了').catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f().then(v => console.log(v));
// 出错了
// hello world
```

- 多个 await 命令后面的异步操作，如果不存在继发关系，最好让它们同时触发，缩短程序的执行时间。

```js
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

### yield\* 语句

yield\* 语句也可以跟一个异步遍历器。与同不 Generator 函数一样，for await ... of 循环会展开 yield\*。

```js
async function* gen1() {
  yield 'a';
  yield 'b';
  return 2;
}

async function* gen2() {
  // result 最终会等于 2
  const result = yield* gen1();
}

// for await...of
(async function() {
  for await (const x of gen2()) {
    console.log(x);
  }
})();
// a
// b
```
