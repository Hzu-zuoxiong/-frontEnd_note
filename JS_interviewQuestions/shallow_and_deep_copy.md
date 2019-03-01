# 深浅拷贝

&emsp;&emsp;我们可以知道，如果给一个变量赋值一个对象，那么两者的值会是同一个引用，其中一方改变，另一方也会相应改变。通常在开发中我们不希望出现这样的问题，我们可以使用浅拷贝来解决这个问题。

## 浅拷贝

1. 通过 `Object.assign` 解决

```js
let a = {
    age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1
```
2. 通过展开运算符(`...`)解决

```js
let a = {
    age: 1
}
let b = {...a}
a.age = 2
console.log(b.age) // 1
```

&emsp;&emsp;通常浅拷贝就能解决大部分问题了，但是浅拷贝只解决了第一层的问题，如果接下去的值中还有对象的话，两者依旧享有相同的引用。要解决这个问题，我们需要引入深拷贝。

## 深拷贝

1. 通过 `JSON.parse(JSON.stringify(object))` 解决

```js
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE

// 在遇到函数、 undefined 或者 symbol 的时候，该对象也不能正常的序列化
let a = {
    age: undefined,  // 忽略 undefined
    sex: Symbol('male'),
    jobs: function() {},
    name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"}
```

局限性：
 
- 会忽略 `undefined`
- 会忽略 `symbol`
- 不能序列化函数
- 不能解决循环引用的对象

通常情况下，这个函数可以解决大部分问题，并且该函数是内置函数中处理深拷贝性能最快的。如果你所需拷贝的对象含有内置类型并且不包含函数，可以使用 `MessageChannel`。

```js
function structuralClone(obj) {
  return new Promise(resolve => {
    const {port1, port2} = new MessageChannel();
    port2.onmessage = ev => resolve(ev.data);
    port1.postMessage(obj);
  });
}

var obj = {
  a: 1, 
  b: {
    c: b
  },
  e: undefined
}
// 注意该方法是异步的
// 可以处理 undefined 和循环引用对象
(async () => {
  const clone = await structuralClone(obj) // {a: 1, b: { c : b }, e: undefined}
})()
```

常规使用深拷贝时的写法。

```js
function deepClone(obj) {
    var buf;
    if(obj instanceof Array) {
        buf = [];
        var i = obj.length;
        while(i--) {
            buf[i] = deepClone(obj[i]);
        }
        return buf;
    } else if(obj instanceof Object) {
        buf = {};
        for(var k in obj) {
            buf[k] = deepClone(obj[k]);
        }
        return buf;
    } else {
        return obj;
    }
}
```