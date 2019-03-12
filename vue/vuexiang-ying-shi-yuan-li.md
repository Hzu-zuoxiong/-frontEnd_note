# Vue 响应式原理

`Vue`是基于`Object.defineProperty`实现数据响应的，`Vue`通过`Object.defineProperty`的`getter/setter`对收集的依赖项进行监听，在属性被访问和修改时通知变化，进而更新视图数据。

```js
let data = { price: 5, quantity: 2 };
let target = null;

// 为每个变量设置一个Dep类，并且封装了需要监视更新的匿名函数
class Dep {
  constructor() {
    this.subscribers = [];
  }

  depend() {
    if (target && !this.subscribers.includes(target)) {
      this.subscribers.push(target);
    }
  }

  notify() {
    this.subscribers.forEach(sub => sub());
  }
}

Object.keys(data).forEach(key => {
  let internalValue = data[key];

  const dep = new Dep();

  Object.defineProperty(data, key, {
    // Get => 记住这个匿名函数，当我们的值变化时，会再次运行它
    get() {
      dep.depend();
      return internalValue;
    },
    // Set => 运行保存的匿名函数，我们的值刚改变
    set(newVal) {
      internalValue = newVal;
      dep.notify();
    }
  });
});

// 创建一个观察程序管理我们正在运行的代码
function watcher(myFunc) {
  target = myFunc;
  target();
  target = null;
}

watcher(() => {
  data.total = data.price * data.quantity;
});
console.log(data.total); // 10
data.price = 20;
console.log(data.total); // 40
data.quantity = 5;
console.log(data.total); // 100
```
