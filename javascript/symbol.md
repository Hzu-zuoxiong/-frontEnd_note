# Symbol

## 简介

`ES5`的对象属性名都是字符串，容易造成冲突。`ES6`引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是`JavaScript`语言的第七种数据类型。前六种分别为：`undefined`、`null`、`Boolean`、`String`、`Number`、`Object`。

`Symbol`值通过`Symbol`函数生成。凡是属性名属于`Symbol`类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。`Symbol`函数可以接受一个字符串作为参数，表示对`Symbol`实例的描述。主要是为了在控制台显式，或者转为字符串时，比较容易区分。如果`Symbol`的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后才生成一个`Symbol`值。

注：`Symbol`**函数前不能使用**`new`**命令**，否则会报错。因为生成的`Symbol`是一个原始类型的值，不是对象。也就是，`Symbol`值不是对象，所以不能添加属性。是一种类似于字符串的数据类型。`Symbol`函数的参数只表示当前`Symbol`值的描述，**相同参数的**`Symbol`**函数的返回值是不相等的**。`Symbol`**值不能于其他类型的值进行运算**，会报错。`Symbol`**值可以转为布尔值，但不能转为数值**。

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

由于每个`Symbol`值是不相等的，这意味着`Symbol`值可以作为标识符，能保证不会出现同名的属性。`Symbol`**值作为对象属性名时，不能用点运算符**。因为点运算符后面总是字符串，所以不会读取作为标识符的`Symbol`值，导致对象属性名实际上是一个字符串，而不是一个`Symbol`值。同理，**对象的内部使用**`Symbol`**值定义属性，必须放在方括号之内**。如果不放在方括号，该属性的键名就是字符串，而不是`Symbol`值。`Symbol`值作为属性名时，该属性是公开属性，而不是私有属性。

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
  enum: 2,
  nonEnum: 3
};

Reflect.ownKeys(obj)
//  ["enum", "nonEnum", Symbol(my_key)]
```

## Symbol.for\(\)、Symbol.keyFor\(\)

`Symbol.for`方法接受一个字符串作为参数，然后搜索有没有以该参数作为名称的`Symbol`值。如果有，返回这个`Symbol`值，否则就新建并返回一个以该字符串为名称的`Symbol`值。`Symbol.for()`与`Symbol()`这两种写法，都会生成新的 `Symbol`。区别是，前者会被登记在全局环境中供搜索，后者不会。`Symbol.for()`不会每次调用就返回一个新的 `Symbol`类型的值，而是会先检查给定的`key`是否已经存在，如果不存在才会新建一个值。

```js
Symbol.for("bar") === Symbol.for("bar")
// true

// 每次调用Symbol()都会返回一个不同的值
Symbol("bar") === Symbol("bar")
// false
```

`Symbol.keyFor`方法返回一个已登记的 `Symbol`类型值的`key`。

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

// Symbol()写法没有登记机制
// 则变量s2属于未登记的 Symbol 值
let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

注意，`Symbol.for`**为 **`Symbol`**值登记的名字，是全局环境的**，可以在不同的 iframe 或 service worker 中取到同一个值。

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

对象的`Symbol.isConcatSpreadable`属性等于一个布尔值，表示该对象用于`Array.prototype.concat()`时，是否可以展开。

```js
// 数组默认可以展开
let arr1 = ['c', 'd'];
['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']
arr1[Symbol.isConcatSpreadable] // undefined

let arr2 = ['c', 'd'];
arr2[Symbol.isConcatSpreadable] = false;
['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']

// 对象默认不可以展开
let obj = {length: 2, 0: 'c', 1: 'd'};
['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']

obj[Symbol.isConcatSpreadable] = true;
['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
```

上面代码表明，**数组的默认行为是可以展开的**，`Symbol.isConcatSpreadable`默认等于`undefined`。该属性等于`true`时，也有展开效果。**类似数组的对象正好相反，默认不展开**。它的`Symbol.isConcatSpreadable`属性设为`true`，才可以展开。

### Symbol.species

对象的`Symbol.species`属性，指向一个构造函数。创建衍生对象时，会使用该属性。

```js
class MyArray extends Array {
}

const a = new MyArray(1, 2, 3);
const b = a.map(x => x);
const c = a.filter(x => x > 1);

b instanceof MyArray // true
c instanceof MyArray // true

// 默认Symbol.species属性等同于下面的写法
static get [Symbol.species]() {
  return this;
}

// 修改Symbol.species后
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}

const a = new MyArray();
const b = a.map(x => x);

b instanceof MyArray // false
b instanceof Array // true
```

上面代码，`b`和`c`都是调用数组方法生成的，所以应该是数组（`Array`的实例），但实际上它们也是`MyArray`的实例。修改`Symbol.species`属性后，其衍生对象`b`就不是`MyArray`的实例，而是`Array`的实例。

`Symbol.species`的作用在于，实例对象在运行过程中，需要再次调用自身的构造函数时，会调用该属性指定的构造函数。它的用途是，有些类库是在基类的基础上修改的，那么子类使用继承的方法时，可能希望返回基类的实例，而不是子类的实例。

### Symbol.match

对象的`Symbol.match`属性，指向一个函数。当指向`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

```js
String.prototype.match(regexp)
// 等同于
regexp[Symbol.match](this)

class MyMatcher {
  [Symbol.match](string) {
    return 'hello world'.indexOf(string);
  }
}

'e'.match(new MyMatcher()) // 1
```

### Symbol.replace

对象的`Symbol.replace`属性，指向一个方法，当该对象被`String.prototype.replace`方法调用时，会返回该方法的返回值。

```js
String.prototype.replace(searchValue, replaceValue)
// 等同于
searchValue[Symbol.replace](this, replaceValue)

const x = {};
x[Symbol.replace] = (...s) => console.log(s);

'Hello'.replace(x, 'World') // ["Hello", "World"]
```

### Symbol.search

对象`Symbol.search`属性，指向一个方法，当该对象被`String.prototype.search`方法调用时，会返回该方法的返回值。

```js
String.prototype.search(regexp)
// 等同于
regexp[Symbol.search](this)

class MySearch {
  constructor(value) {
    this.value = value;
  }
  [Symbol.search](string) {
    return string.indexOf(this.value);
  }
}
'foobar'.search(new MySearch('foo')) // 0
```

### Symbol.split

对象的`Symbol.split`属性，指向一个方法，当该对象被`String.prototype.split`方法调用时，会返回该方法的返回值。

```js
String.prototype.split(separator, limit)
// 等同于
separator[Symbol.split](this, limit)

// 示例
class MySplitter {
  constructor(value) {
    this.value = value;
  }
  [Symbol.split](string) {
    let index = string.indexOf(this.value);
    if (index === -1) {
      return string;
    }
    return [
      string.substr(0, index),
      string.substr(index + this.value.length)
    ];
  }
}

'foobar'.split(new MySplitter('foo'))
// ['', 'bar']

'foobar'.split(new MySplitter('bar'))
// ['foo', '']

'foobar'.split(new MySplitter('baz'))
// 'foobar'
```

### Symbol.iterator

对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法。对象进行`for...of`循环时，会调用`Symbol.iterator`方法，返回该对象默认遍历器。

```js
const myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

### Symbol.toPrimitive

对象的Symbol.toPrimitive属性，指向一个方法。该对象被转为原始类型的值时，会调用这个对象，返回该对象对应的原始类型值。

Symbol.toPrimitive被调用时，会接受一个字符串参数，表示当前运算的模式，一共三种模式。

* Number：该场合需要转成数值。
* 
```

```



