# this、apply、call、bind

## this的指向

在ES5中，**this永远指向最后调用它的那个对象**。this的指向并不是在创建的时候就确定的。

```js
var name = "windowsName";
function fn() {
    var name = 'Inner';
    innerFunction();
    function innerFunction() {
        console.log(this.name);
    }
}
fn() //windowsName
```

fn\(\)是由window调用的，所以this指向的是window。

## 改变this的指向

1. 使用ES的箭头函数
2. 在函数内部使用\_this = this
3. 使用apply、call、bind
4. new实例化一个对象

### 箭头函数

**箭头函数的this始终指向函数定义时的this，而非执行时。**箭头函数中没有this绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this，否则this为undefined。

```js
var name = "windowsName";
var a = {
    name : "innerName",
    func1: function () {
        console.log(this.name)     
    },
    func2: function () {
        setTimeout( () => {
            this.func1()
        },100);
    }
};
a.func2()     // innerName
```

### 使用apply、call、bind

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

## apply与call的区别

* call\(\)方式接收的是若干个参数列表
* apply\(\)接收的是包含多个参数的数组

```js
var a ={
    name : "Cherry",
    fn : function (a,b) {
        console.log( a + b)
    }
}

var b = a.fn;
b.apply(a, [1, 2]);     // 3
b.call(a, 1, 2);        // 3
```

## bind和apply、call区别

bind方法返回值是函数，bin方法不会立即执行，而是返回一个改变了上下文this后的函数。而原函数中的this并没有被改变，依旧指向全局window。

```js
var obj = {
    name: 'Dot'
}

function printName() {
    console.log(this.name)
}

var dot = printName.bind(obj)
console.log(dot) // function () { … }
dot()  // Dot
```

低版本浏览器没有bind方法，可以自己实现：

```js
Function.prototype.bind = function() {
    var self = this;                // 保存原函数
    var context = [].shift.call(arguments);  // 保存需要绑定的this上下文
    var args = [].slice.call(arguments);   // 保存剩余参数，转成数组形式
    return function() {
        self.apply(context, [].concat.call(args, [].slice.call(arguments)));
    }
}
```

## 总结：

call、apply和bind函数的区别：

bind返回对应函数，便于稍后调用；apply、call则是立即调用。除此之外，在ES6的箭头函数下，call和apply将失效。

