# JS继承

## 一、构造继承

通过使用`call、apply`方法可以在新创建的对象上执行构造函数，用父类的构造函数来增加子类的实例。

```js
// 子类
function Sub() {
    Super.call(this);
    this.property = 'Sub Property'
}
```

**优缺点：**

* 简单明了，直接继承超类构造函数的属性和方法
* 无法继承原型链上的属性和方法

## 二、原型链继承

利用原型链来实现继承，超类的一个实例作为子类的原型。

```js
// 子类
function Sub() {
    this.prototype = 'Sub Property';
}
Sub.prototype = new Super();
// 注：这里new Super()生成的超类对象并没有constructor属性，故需添加上
Sub.prototype.constructor = Sub;
```

**优缺点：**

* 实例是子类的实例，实际上也是父类的实例
* 父类新增原型方法和属性，子类都能访问到
* 所有子类的实例的原型都共享同一个超类实例的属性和方法
* 无法实现多继承

## 三、组合继承

利用构造继承和原型链组合

```js
// 子类
function Sub() {
    Super.call(this);
    this.property = 'Sub Property'
}
Sub.prototype = new Super();
// 注：这里new Super()生成的超类对象并没有constructor属性，故需添加上
Sub.prototype.constructor = Sub;
```

**优缺点：**

* 解决了构造继承和原型链继承的两个问题
* 实际上子类会拥有超类的两份属性，只是子类的属性覆盖了超类的属性

## 四、原型式继承

采用原型式继承并不需要定义一个类，传入参数`obj`，生成一个继承`obj`对象的对象

```js
function objectCreate(obj) {
    function F() {}
    F.prototype = obj;
    return new F();
}
```

**优缺点：**

* 直接通过对象生成一个继承该对象的对象
* 不是类式继承，而是原型式基础，缺少类的概念

## 五、寄生式继承

创建一个仅仅用于封装继承过程的函数，然后在内部以某种方式增强对象，最后返回对象

```js
function objectCreate(obj) {
    function F() {}
    F.prototype = obj;
    return new F();
}

function createSubObj(superInstance) {
    var clone = objectCreate(superInstance);
    clone.property = 'Sub Property';
    return clone;
}
```

**优缺点：**

* 原型式继承的一种拓展
* 依旧没有类的概念

## 六、寄生组合式继承

结合寄生式继承和组合式继承，完美实现不带两份超类属性的继承方式

```js
function inheritPrototype(Super, Sub) {
    var superProtoClone = Object.create(Super.prototype);
    superProtoClone.constructor = Sub;
    Sub.prototype = Super;
}

function Sub() {
    Super.call();
    Sub.property = 'Sub Property'
}
inheritPrototype(Super, Sub);
```

**优缺点：**

* 完美实现继承，解决了组合式继承带两份属性的问题
* 过于繁琐，故不如组合继承



