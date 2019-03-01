# `this、apply、call、bind`

## `this` 的指向

&emsp;&emsp;在 `ES5` 中，**`this` 永远指向最后调用它的那个对象**。`this` 的指向并不是在创建的时候就确定的。

```js
var name = 'windowsName';
function fn() {
  var name = 'Inner';
  innerFunction();
  function innerFunction() {
    console.log(this.name);
  }
}
fn(); //windowsName
```

&emsp;&emsp;`fn()` 是由 `window` 调用的，所以 `this` 指向的是 `window`。

## 改变 `this` 的指向

1. 使用 `ES` 的箭头函数
2. 在函数内部使用 `_this = this`
3. 使用 `apply、call、bind`
4. `new` 实例化一个对象

### 箭头函数

&emsp;&emsp;**箭头函数的 `this` 始终指向函数定义时的 `this`，而非执行时。** 箭头函数中没有 `this` 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 `this` 绑定的是最近一层非箭头函数的 `this`，否则 `this` 为 `undefined`。

```js
var name = 'windowsName';
var a = {
  name: 'innerName',
  func1: function() {
    console.log(this.name);
  },
  func2: function() {
    setTimeout(() => {
      this.func1();
    }, 100);
  }
};
a.func2(); // innerName
```

### 使用 `apply、call、bind`

```js
// 使用apply
func2: function () {
    setTimeout(function () {
        this.func1()
    }.apply(a),100);
}

// 使用call
func2: function () {
    setTimeout(  function () {
        this.func1()
    }.call(a),100);
}

// 使用bind
func2: function () {
    setTimeout(  function () {
        this.func1()
    }.bind(a)(),100);
}
```

## `call` 与 `apply` 的区别

- `call()` 方式接收的是若干个参数列表
- `apply()` 接收的是包含多个参数的数组

```js
var a = {
  name: 'Cherry',
  fn: function(a, b) {
    console.log(a + b);
  }
};

var b = a.fn;
b.apply(a, [1, 2]); // 3
b.call(a, 1, 2); // 3
```

### 模拟实现 `call`

- 不传入第一个参数，那么默认为 `window`
- 改变了 `this` 指向，让新的对象可以执行该函数。即，给新的对象添加一个函数，然后在执行完以后删除。

```js
Function.prototype.myCall = function(context) {
  var context = context || window;
  // 给 context 添加一个属性
  // getValue.call(a, 'yck', '24') => a.fn = getValue
  context.fn = this;
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1);
  // getValue.call(a, 'yck', '24') => a.fn('yck', '24')
  var result = context.fn(...args);
  // 删除 fn
  delete context.fn;
  return result;
};
```

### 模拟实现 `apply`

与 `call` 的思路类似

```js
Function.prototype.myApply = function(context) {
  var context = context || window;
  context.fn = this;

  var result;
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }

  delete context.fn;
  return result;
};
```

## `bind` 和 `apply、call` 区别

&emsp;&emsp;`bind` 方法返回值是函数，`bind` 方法不会立即执行，而是返回一个改变了上下文 `this` 后的函数。而原函数中的 `this` 并没有被改变，依旧指向全局 `window`。

```js
var obj = {
  name: 'Dot'
};

function printName() {
  console.log(this.name);
}

var dot = printName.bind(obj);
console.log(dot); // function () { … }
dot(); // Dot
```

低版本浏览器没有 `bind` 方法，可以自己实现：

```js
Function.prototype.myBind = function() {
  var self = this; // 保存原函数
  var context = [].shift.call(arguments); // 保存需要绑定的this上下文
  var args = [].slice.call(arguments); // 保存剩余参数，转成数组形式
  return function() {
    self.apply(context, [].concat.call(args, [].slice.call(arguments)));
  };
};
```

## 总结：

`call、apply` 和 `bind` 函数的区别：

`bind` 返回对应函数，便于稍后调用；`apply、call` 则是立即调用。除此之外，在 `ES6` 的箭头函数下，`call` 和 `apply` 将失效。
