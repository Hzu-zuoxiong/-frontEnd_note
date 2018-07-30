# Class

## 简介

ES6的class可以看作一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰，更像面向对象编程的语法。

类和模块的内部，默认就是严格模式，不需要使用use strict指定运行模式。

定义类的方法的时候，前面不需要加上function关键字，且方法之间不需要逗号分隔，否则会报错。

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



