# Symbol

## 简介

`ES5`的对象属性名都是字符串，容易造成冲突。`ES6`引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是`JavaScript`语言的第七种数据类型。前六种分别为：`undefined`、`null`、`Boolean`、`String`、`Number`、`Object`。

`Symbol`值通过`Symbol`函数生成。凡是属性名属于`Symbol`类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。`Symbol`函数可以接受一个字符串作为参数，表示对`Symbol`实例的描述。主要是为了在控制台显式，或者转为字符串时，比较容易区分。如果`Symbol`的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个`Symbol`值。

注：**`Symbol`函数前不能使用`new`命令**，否则会报错。因为生成的`Symbol`是一个原始类型的值，不是对象。也就是，`Symbol`值不是对象，所以不能添加属性。是一种类似于字符串的数据类型。`Symbol`函数的参数只表示当前`Symbol`值的描述，**相同参数的`Symbol`函数的返回值是不相等的**。**`Symbol`值不能于其他类型的值进行运算**，会报错。**`Symbol`值可以转为布尔值，但不能转为数值**。

```js
let s = Symbol('foo');

s // Symbol(foo)
s.toString() // "Symbol(foo)"
typeof s;   // "symbol"

// 没有参数的情况
let s1 = Symbol();
let s2 = Symbol();

s1 === s2 // false

// 有参数的情况
let s1 = Symbol('foo');
let s2 = Symbol('foo');

s1 === s2 // false

let sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string

Boolean(sym) // true
!sym  // false

Number(sym) // TypeError
sym + 2 // TypeError
```

## 作为属性名的`Symbol`

由于每个`Symbol`值是不相等的，这意味着`Symbol`值可以作为标识符，能保证不会出现同名的属性。**`Symbol`值作为对象属性名时，不能用点运算符**。因为点运算符后面总是字符串，所以不会读取作为标识符的`Symbol`值，导致对象属性名实际上是一个字符串，而不是一个`Symbol`值。同理，**对象的内部使用`Symbol`值定义属性，必须放在方括号之内**。如果不放在方括号，该属性的键名就是字符串，而不是`Symbol`值。`Symbol`值作为属性名时，该属性是公开属性，而不是私有属性。

```js
const mySymbol = Symbol();
const a = {};

a.mySymbol = 'Hello!';
a[mySymbol] // undefined
a['mySymbol'] // "Hello!"

let s = Symbol();

// 对象内部使用Symbol值需要方括号
let obj = {
  [s]: function (arg) { ... }
};
```

## 属性名的遍历

`Symbol`作为属性名，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。但是，它也不是私有属性，有一个`Object.getOwnPropertySymbols`方法，可以获取指定对象的所有`Symbol`属性名。`Object.getOwnPropertySymbols`

方法返回一个数组，成员是当前对象的所有用作属性名的 `Symbol` 值。另一个新的API，`Reflect.ownKeys`方法可以返回所有类型的键名，包括常规键名和`Symbol`键名。

```js
const obj = {};
let foo = Symbol("foo");
Object.defineProperty(obj, foo, {
  value: "foobar",
});

for (let i in obj) {
  console.log(i); // 无输出
}
Object.getOwnPropertyNames(obj)
// []
Object.getOwnPropertySymbols(obj)
// [Symbol(foo)]

// 使用Reflect。ownKeys遍历键名
let obj = {
  [Symbol('my_key')]: 1,
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

## Symbol.for\(\)、Symbol.keyFor\(\)

`Symbol.for`方法接受一个字符串作为参数，然后搜索有没有以该参数作为名称的`Symbol`值。如果有，返回这个`Symbol`值，否则就新建并返回一个以该字符串为名称的`Symbol`值。`Symbol.for()`与`Symbol()`这两种写法，都会生成新的 `Symbol`。区别是，前者会被登记在全局环境中供搜索，后者不会。`Symbol.for()`不会每次调用就返回一个新的 `Symbol `类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值。

```js
Symbol.for("bar") === Symbol.for("bar")
// true

// 每次调用Symbol()都会返回一个不同的值
Symbol("bar") === Symbol("bar")
// false
```

`Symbol.keyFor`方法返回一个已登记的 `Symbol `类型值的`key`。

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

// Symbol()写法没有登记机制
// 则变量s2属于未登记的 Symbol 值
let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

注意，**`Symbol.for`为 `Symbol `值登记的名字，是全局环境的**，可以在不同的 iframe 或 service worker 中取到同一个值。

## 内置的Symbol值

ES6提供了11个内置的`Symbol`值，指向语言内部使用的方法。

### Symbol.hasInstance

对象的`Symbol.hasInstance`属性，指向一个内部方法。当其他对象使用`instanceof`运算符，判断是否为该对象的实例时，会调用这个方法。比如，`foo instanceof Foo`在语言内部，实际调用的是`Foo[Symbol.hasInstance](foo)`。

```js
const Even = {
  [Symbol.hasInstance](obj) {
    return Number(obj) % 2 === 0;
  }
};

1 instanceof Even // false
2 instanceof Even // true
```

### Symbol.isConcatSpreadable

对象的

