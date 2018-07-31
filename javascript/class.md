# Class

## 简介

ES6的class可以看作一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰，更像面向对象编程的语法。ES6的类只是ES5的构造函数的一层包装，所以函数的许多特性都被class继承，包括name属性。name属性总是返回紧跟在class关键字后面的类名。类的所有实例共享一个原型对象，与ES5一样。

类和模块的内部，**默认就是严格模式**，不需要使用use strict指定运行模式。**类必须使用new调用**，否则会报错，这与普通的构造函数有所不同。**类不存在变量提升**，与ES5完全不同。

定义类的方法的时候，前面**不需要加上function关键字**，且方法之间**不需要逗号分隔**，否则会报错。

```js
//定义类
class Point {
  constructor() {
    // ...
  }
  toString() {
    // ...
  }
  toValue() {
    // ...
  }
  // 类的属性名可以用表达式表示
  [methodName]() {
    // ...
  }
}

// 等同于
Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};

// 类的所有方法都定义在类的prototype属性上
// 可以使用Object.assgin方法一次性添加多个方法
Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});

// 类的数据原型就是函数
typeof Point // "function"
Point === Point.prototype.constructor // true

// 类内部定义的所有方法都是不可枚举的
Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]

class B {}
let b = new B();

// 类的实例上调用方法，就是调用原型上的方法
b.constructor === B.prototype.constructor // true
```

上面代码表明：

* 类的数据类型就是函数，类本身就指向构造函数。
* 类的所有方法都定义在类的prototype属性上面。
* 在类的实例上面调用方法，就是调用原型上的方法。
* 类的内部定义的所有方法都是不可枚举的，与ES5的行为不一致。
* 类的属性名可以用表达式表示

## constructor方法

constructor方法是类的默认方法，通过new命令生成对象实例时自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的**constructor方法会被默认添加**。constructor方法默认返回实例对象。

```js
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

## Class 表达式

与函数一样，类也可以使用表达式的形式定义。采用Class表达式，可以写出立即执行的Class。

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};


// 立即执行
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```

上面代码使用表达式定义了一个类。这个类的名字是MyClass而不是Me，Me只在Class的内部代码使用，指代当前类。

## this的指向

类的方法内部如果含有this，**默认指向类的实例**，但单独使用该方法，很可能报错。

```js
class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

## Class的取值函数（getter）和存值函数（setter）

与ES5一样，在”类“的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。存值函数和取值函数是设置在属性的Descriptor对象上。

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

## Class的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，**加static关键字**，就表示该方法不会被实例继承，而是直接通过类来调用，称为”静态方法“。**父类的静态方法，可以被子类继承**。父类的静态方法也可以从super对象上调用。

**注：如果静态方法包含this关键字，这个this指的是类，而不是实例。**

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
  static bar () {
    this.baz();
  }
  static baz () {
    console.log('hello');
  }
  baz () {
    console.log('world');
  }
}

Foo.classMethod() // 'hello'
Foo.bar() // hello

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

## Class的静态属性和实例属性

静态属性指的是Class本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

* ### 类的实例属性

以前定义实例属性，只能写在类的constructor方法里面。而新的写法可以不在constructor方法里面定义。

```js
class MyClass {
  myProp = 42;
  state = {
    count: 0
  };
  constructor() {
    console.log(this.myProp); // 42
  }
}
```

* ### 类的静态属性

类的静态属性只要在实例属性的写法前面**加上static关键字**即可。

```js
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // 42
  }
}
```

## new.target属性

ES6为new命令引入了一个new.target属性，该属性一般用在构造函数之中，返回new命令作用于的那个构造函数。如果构造函数不是通过new命令调用的，new.target会返回undefined，因此这个属性可以用来确定构造函数是怎么调用的。

```js
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}
var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

Class内部调用new.target，返回当前Class。**注：子类继承父类时，new.target会返回子类。在函数外部使用new.target会报错。**

## Class的继承

### 简介

Class可以通过extends关键字实现继承，比ES5的通过修改原型链要清晰方便。**super关键字表示父类的构造函数**，用来新建父类的this对象。**子类必须在constructor方法中调用super方法**，否则新建实例时会报错。因为子类的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。

ES5中的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面。ES6的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），再用子类的构造函数修改this。

可以用Object.getPrototypeOf\(\)判断一个类是否继承了另一个类。

```js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}

Object.getPrototypeOf(ColorPoint) === Point
// true
```

### super关键字

super关键字，**既可以当作函数使用，也可以当作对象使用**。作为函数时，super\(\)只能用在子类的构造函数之中，用在其他会报错。

* super作为函数调用时，代表父类的构造函数。（**子类的构造函数必须执行一次super函数**）。super虽然代表父类的构造函数，但返回的是子类的实例，即**super内部的this指向子类**。因此super\(\)相当于A.prototype.constructor.call\(this\)。
* super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。（super指向父类的原型对象，所以**定义在父类实例上的方法或属性是无法通过super调用的**）。

```js
class A {
  constructor() {
    this.p = 2;
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    super();
  }
  get m() {
    return super.p;
  }
}
// super内部的this指向子类
new A() // A
new B() // B

// 父类实例的属性和方法无法获取
let b = new B();
b.m // undefined

```

super内部的this指向当前子类实例，如果**通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性**。

```js
class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}
```

注：使用super的时候，**必须显式指定是作为函数、还是作为对象使用**，否则会报错。

```js
console.log(super); // 报错
console.log(super.valueOf() instanceof B); // true
```

由于对象总是继承其他对象的，所以**可以在任意一个对象中，使用super关键字**。

### 原生构造函数的继承

原生构造函数是指语言内置的构造函数，通常用来生成数据结构。ECMAScript的原生构造函数：

* Boolean\(\)
* Number\(\)
* String\(\)
* Array\(\)
* Date\(\)
* Function\(\)
* RegExp\(\)
* Error\(\)
* Object\(\)

以前，原生构造函数是无法继承的。因为子类无法获得原生构造函数的内部属性，通过Array.apply\(\)或者分配给原型对象都不行。原生构造函数会忽略apply方法传入的this，也就是原生构造函数的this无法绑定，导致无法获取内部属性。ES5 是先新建子类的实例对象this，再将父类的属性添加到子类上，由于父类的内部属性无法获取，导致无法继承原生的构造函数。

ES6允许继承原生构造函数定义子类，因为ES6是先新建父类的实例对象this，然后再用子类的构造函数修饰this，使得父类的所有行为都可以继承。

```js
class MyArray extends Array {
  constructor(...args) {
    super(...args);
  }
}

var arr = new MyArray();
arr[0] = 12;
arr.length // 1

arr.length = 0;
arr[0] // undefined
```

注：继承Object的子类，有一个行为差异。

```js
class NewObj extends Object{
  constructor(){
    super(...arguments);
  }
}
var o = new NewObj({attr: true});
o.attr === true  // false
```

上面代码中，`NewObj`继承了`Object`，但是无法通过`super`

方法向父类

`Object`

传参。这是因为 ES6 改变了

`Object`

构造函数的行为，一旦发现

`Object`

方法不是通过

`new Object()`

这种形式调用，ES6 规定

`Object`

构造函数会忽略参数。

