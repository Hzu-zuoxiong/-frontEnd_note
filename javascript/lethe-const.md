# let和const

## Let命令 {#let命令}

### 基本用法 {#基本用法}

ES6新增了let命令，用于声明变量，用法类似于var，但声明的变量只在let命令所在的代码块内有效。很适用for循环的计数器。  
另外，**for循环设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域**。

```js
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
//总共输出三次'abc'
//abc
//abc
```

上面代码表明函数内部的变量 i 与循环变量 i 不在同一个作用域，而是有各自的作用域。

### 不存在变量提升 {#不存在变量提升}

var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined。而let命令所声明的变量一定要在声明之后使用，否则将会报错。

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

### 暂时性死区 {#暂时性死区}

在代码块内，使用let命令声明变量之前，该变量都是不可用的。在语法上称之为“暂时性死区”（简称TDZ）

```js
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

有些“死区”较为隐蔽。

```js
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

报错是因为参数x的默认值为另一个参数，而此时y还未声明，属于“死区”。  
总之，暂时性死区的本质就是，只要进入当前作用域，所要使用的变量就已存在，但不可获取，只有等到变量声明的那行代码出现，才可以获取和使用该变量。

### 不允许重复声明 {#不允许重复声明}

let命令不允许在相同作用域内声明同一变量。函数内部也就不可以重新声明参数。

```js
function func(arg) {
  let arg; // 报错
}
function func(arg) {
  {
    let arg; // 不报错
  }
}
```

## 块级作用域 {#块级作用域}

ES6的块级作用域允许声明函数的规则只在使用大括号的情况下成立，如果没有使用大括号则会报错。

```js
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```

## const命令 {#const命令}

### 基本用法 {#基本用法-1}

const声明一个只读常量。一旦声明，常量的值就不能改变。const一旦声明常量，就必须立即初始化，不能留到以后赋值。

```js
const foo;
// SyntaxError: Missing initializer in const declaration
```

* 对于const而言，只声明不赋值就会报错。
* const的作用域与let命令相同，只在声明所在的块级作用域内有效，声明的常量也不会提升，同样存在暂时性死区。

### 本质 {#本质}

const实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。对于复合类型的数据（主要是对象和数组）而言，变量指向的内存地址保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，这完全不能控制。

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

如果想将对象冻结，应该使用Object.freeze方法。

```js
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

除了将对象本身冻结，对象的属性也应该冻结。

```js
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

## 顶层对象的属性

顶层对象在浏览器环境中指的是windows对象，在Node环境中指的是global对象。在ES5中，顶层对象的属性与全局变量是等价的。  
ES6为了保持兼容性，var命令和function命令声明的全局变量依旧是顶层对象的属性；另外，let命令、const命令、class命令声明的全局变量不属于顶层对象的属性。

```js
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```



