# 14 道 JavaScript 题

这 14 道`JavaScript`题来自[http://perfectionkills.com/javascript-quiz/](http://perfectionkills.com/javascript-quiz/)

1.

```js
(function() {
  return typeof arguments;
})();
```

答案：`"object"`

解析：`arguments`是类数组对象，就算是数组，`typeof`返回的也是`"object"`

2.

```js
var f = function g() {
  return 23;
};
typeof g();
```

答案：`Uncaught ReferenceError: g is not defined`

解析：在`JS`中，函数声明有两种：

- 函数声明：`function fn() {...};`
- 函数表达式：`var fn = function() {...};`

类似`var foo = function bar() {...}`同一按函数表达式处理，函数外部无法通过 bar 访问函数。

3.

```js
(function(x) {
  delete x;
  return x;
})(1);
```

答案：1

解析：`delete`操作符用于删除对象的成员变量，不能删除函数的参数。

4.

```js
var y = 1,
  x = (y = typeof x);
x;
```

答案：`"undefined"`

解析：先定义`y`并赋值为 1，然后将`typeof x` 赋值给`y`，此时`y`为`"undefined"`，最后将`y`赋值给`x`。

5.

```js
(function f(f) {
  return typeof f();
})(function() {
  return 1;
});
```

答案：`"number"`

解析：函数里的`f()`是参数 f 的执行结果，也就是`typeof 1`，最终结果返回`"number"`。

6.

```js
var foo = {
  bar: function() {
    return this.baz;
  },
  baz: 1
};
(function() {
  return typeof arguments[0]();
})(foo.bar);
```

答案：`"undefined"`

解析：此时，`this`指向的是`arguments`，而`arguments`中没有`baz`变量，则答案为`"undefined"`。

![](/assets/this指向arguments.png)

7.

```js
var foo = {
  bar: function() {
    return this.baz;
  },
  baz: 1
};
typeof (f = foo.bar)();
```

答案：`"undefined"`

解析：此时`this`指向全局`Window`，而全局`Window`没有`baz`变量，返回`"undefined"`。

8.

```js
var f = (function f() {
  return '1';
},
function g() {
  return 2;
})();
typeof f;
```

答案：`"number"`

解析：逗号操作符多用于声明多个变量；但除此之外，逗号操作符还可以用于赋值。在用于赋值时，逗号操作符总会返回表达式中的最后一项。故此，只有最后面的函数会被执行。

9.

```js
var x = 1;
if (function f() {}) {
  x += typeof f;
}
x;
```

答案：`"1undefined"`

解析：括号内的`function f(){}`不是函数声明，会被转换为`true`，因此`f`未定义。

10.

```js
var x = [typeof x, typeof y][1];
typeof typeof x;
```

答案：`"undefined"`

解析：第一句代码执行完`x==="undefined"`，所以连续两次`typeof`还是`"string"`。

11.

```js
(function(foo) {
  return typeof foo.bar;
})({ foo: { bar: 1 } });
```

答案：`"undefined"`

解析：参数`foo`指的是`{ foo: { bar: 1 } }`，故参数`foo`里面没有`bar`变量，返回`"undefined"`。

12.

```js
(function f() {
  function f() {
    return 1;
  }
  return f();
  function f() {
    return 2;
  }
})();
```

答案：2

解析：函数声明提升，后面的`f()`会覆盖前面的`f()`。

13.

```js
function f() {
  return f;
}
new f() instanceof f;
```

答案：`false`

解析：构造函数不需要显式声明返回值，默认返回`this`值。当显式声明了返回值时，如果返回值是非对象（数字、字符串等），这个返回值会被忽略，继续返回`this`值。但如果返回的是对象，那么这个显式返回值会被返回。

![](/assets/构造函数返回值.png)

14.

```js
with (function(x, undefined) {}) length;
```

答案：2

解析：`with`限定作用域是这个函数，`function.length`返回函数的参数个数，所以是 2。
