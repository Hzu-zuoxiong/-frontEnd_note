# this、apply、call、bind

## this的指向

在ES5中，this永远指向最后调用它的那个对象。this的指向并不是在创建的时候就确定的。

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

箭头函数的this始终指向函数定义时的this，而非执行时。箭头函数中没有this绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this，否则this为undefined。

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



