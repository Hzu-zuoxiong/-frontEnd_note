# 函数的扩展

## 参数默认值

`ES6`之前，不能直接为函数的参数指定默认值。`ES6`允许为函数的参数设置默认值，即直接写在参数定义的后面。指定了默认值之后，函数的`length`属性将返回没有指定默认值的参数个数。也就是，**指定默认值，`length`属性将失真**。如果设置了默认值的参数不是尾参数，那么`length`属性也不再计入后面的参数了。

```js
// ES5
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

// ES6
function log(x, y = 'World') {
  console.log(x, y);
}
```

* 参数变量是默认声明的，不能用`let`或`const`再次声明。
* 使用参数默认值时，函数不能有同名参数。
* 传入`undefined`，将触发参数等于默认值，`null`则没有这个效果。

```js
function foo(x = 5) {
  let x = 1; // error
  const x = 2; // error
}

// 不报错
function foo(x, x, y) {
  // ...
}

// 报错
function foo(x, x, y = 1) {
  // ...
}
// SyntaxError: Duplicate parameter name not allowed in this context

function foo(x = 5, y = 6) {
  console.log(x, y);
}

foo(undefined, null)
// 5 null
```

注：**参数默认值不是传值的**，而是每次都重新计算默认值表达式的值。也就是，参数默认值是惰性求值的。

通常情况下，定义了默认值的参数，应该是函数的尾参数。因为这样比较容易看出可以省略哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。

### 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（`context`）。等到初始化结束，这个作用域就会消失。这种行为，在不设置参数默认值时，是不会出现的。如果参数默认值是一个函数，也遵守这个规则。

```js
var x = 1;
// 调用函数f时，参数形成以个单独的作用域
// 作用域里，默认值变量x指向第一个参数x，而不是全局变量x
function f(x, y = x) {
  console.log(y);
}

f(2) // 2

// 函数调用时，函数体内的局部变量z影响不到默认值变量z
// 此时外层若变量z不存在，则会报错
function f(y = z) {
  let z = 2;
  console.log(y);
}

f() // ReferenceError: z is not defined

// 参数b=b形成单独作用域。实际执行let b=b
// 由于暂时性死区，代码报'b为定义'错误
var b = 1;
function foo(b = b) {
  // ...
}

foo() // ReferenceError: b is not defined
```

## `rest`参数

`ES6`引入`rest`参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象。`rest`参数搭配的变量是一个数组，该变量将多余的参数放入数组中。`arguments`对象不是数组，而是一个类似数组的对象。为了使用数组的方法，必须使用`Array.prototype.slice.call`将其转为数组。`rest`参数就不存在这个问题，数组特有的方法都可以使用。

```js
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```

**注：`rest`参数之后不能再有其他参数，否则会报错**。函数的`length`属性也不包括rest参数。

```js
// 报错
function f(a, ...b, c) {
  // ...
}
```

## 严格模式与`name`属性

### 严格模式

`ES5`函数内部可以设定严格模式，而`ES6`规定只要函数参数使用了默认值、解构赋值或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。

两种方法可以规避这种限制：

1. 设定全局性的严格模式。
2. 把函数包在一个无参数的立即执行函数里面。

### `name`属性

如果将一个匿名函数赋值给一个变量，`ES5`的`name`属性会返回空字符串，而`ES6`的`name`属性会返回实际的函数名。`Function`构造函数返回的函数实例，`name`属性的值为`anonymous`。`bind`返回的函数，`name`属性值会加上`bound`前缀。

```js
var f = function () {};
// ES5
f.name // ""

// ES6
f.name // "f"

(new Function).name // "anonymous"

function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```

## 箭头函数

### 基本用法

`ES6`允许使用“箭头”（`=>`）定义函数。箭头函数使用一个圆括号代表参数部分，如果箭头函数的代码块多于一条语句，就要使用大括号，并且使用`return`语句返回。由于大括号被解释为代码块，所以如果**箭头函数直接返回一个对象，必须在对象外面加上括号**，否则会报错。箭头函数可以与变量结构结合使用。

```js
var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};

// 报错
let getTempItem = id => { id: id, name: "Temp" };

// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });

// 只有一行语句，且不需要返回值，不需要写大括号
let fn = () => void doesNotReturn();

const full = ({ first, last }) => first + ' ' + last;
// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}
```

### 注意点

箭头函数有几个使用注意点：

1. 函数体内的**`this`对象，就是定义时所在的对象**，而不是使用时所在的对象。
2. **不可以当作构造函数**，也就是不能使用`new`命令，否则会抛出一个错误。
3. 不可以使用**`arguments`对象，该对象在函数体内不存在**。
4. **不可以使用`yield`命令**，因此箭头函数不能用作`Generator`函数。

`this`指向的固定化，并不是因为箭头函数内部有绑定`this`的机制，实际原因是箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。正因为它没有`this`，也就不能作构造函数。

箭头函数转`ES5`代码：

```js
// ES6
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

// ES5
function foo() {
  var _this = this;

  setTimeout(function () {
    console.log('id:', _this.id);
  }, 100);
}

// 所有内层函数都是箭头函数，都没有自己的this
// 则this都是指向最外层foo函数的this
function foo() {
  return () => {
    return () => {
      return () => {
        console.log('id:', this.id);
      };
    };
  };
}

var f = foo.call({id: 1});

var t1 = f.call({id: 2})()(); // id: 1
var t2 = f().call({id: 3})(); // id: 1
var t3 = f()().call({id: 4}); // id: 1
```

除了`this`，以下三个变量在箭头函数之中也是不存在的，它们指向外层函数对应的变量：`arguments`、`super`、`new.target`。

```js
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```

## 双冒号运算符

由于箭头函数没有自己的`this`，所以当然不能用`call()`、`apply()`、`bind()`等方法改变this的指向。

函数绑定运算符是并排的两个冒号（`::`），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即`this`对象），绑定到右边的函数上面。若双冒号的左边为空，右边是一个对象的方法，则等于将该方法绑定在该对象上。若双冒号运算符的运算结果，还是一个对象，就可以采用链式写法。 

```js
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);

var method = obj::obj.foo;
// 等同于
var method = ::obj.foo;
```



