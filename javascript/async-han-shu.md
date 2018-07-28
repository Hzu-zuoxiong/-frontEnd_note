# async 函数

ES2017 标准引入了async函数，使得异步操作变得更加方便，它是Generator 函数的语法糖。

async 函数对Generator函数的改进：

1. 内置执行器：Generator函数的执行必须靠执行器，所以才有co模块，而async函数自带执行器。
2. 语义更清晰：async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
3. 更好的适用性：async函数的await命令后面，可以是Promise对象和原始类型的值。
4. 返回值是Promise：Generator函数的返回值是Iterator对象，async函数的返回值是Promise对象。

async函数可以看作多个异步操作，包装成一个Promise对象，而await命令就是内部then命令的语法糖。

async函数的实现原理，就是将Generator函数和自动执行器包装再一个函数里。

```js
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
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
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
```

## 基本用法

async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```js
function timeout(ms) {
  return new Promise((resolve) => {
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

### 返回Promise对象

async函数返回一个Promise对象，内部return语句返回的值，会成为then方法回调函数的参数。async函数内部抛出错误，会导致Promise对象变为reject状态。

### Promise对象的状态变化

async函数必须等到内部所有的await命令后面的Promise对象执行完，才会发生状态改变，除非遇到return语句或抛出错误。

### await命令

* await命令后面是一个Promise对象。如果不是，会被转成一个立即resolve的Promise对象。
* await命令只能用在async函数之中，如果用在普通函数，就会报错。
* 只要一个await语句后面的Promise变为reject，那么整个async函数都会中断执行。（可以使用try...catch或再跟一个catch方法变通）

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
    } catch(err) {}
  }
  console.log(i); // 3
}

test();


// 使用catch
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// 出错了
// hello world
```

* 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发，缩短程序的执行时间。

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

yield\* 语句也可以跟一个异步遍历器。与同不Generator函数一样，for await ... of 循环会展开yield\*。

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
(async function () {
  for await (const x of gen2()) {
    console.log(x);
  }
})();
// a
// b
```



