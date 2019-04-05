# JavaScript 考点

- `JS` 的基本数据类型与内置对象

```
基本数据类型：undefined、null、boolean、number、string、symbol

JavaScript 有两种数据类型：原始数据类型和引用数据类型
存储在栈中：原始数据类型（undefined、null、boolean、number、string）
存储在堆中：引用数据类型（对象、数组和函数）

Object是JS中所有对象的父对象
数据封装类对象：Obeject、Array、Boolean、Number、String
其他对象：Function、Arguments、Math、Date、RegExp、Error
```

- 谈谈 cookie 的优缺点

```
优点：极高的扩展性和可用性
1. 通过良好的编程，控制保存在cookie中的session对象的大小
2. 通过加密和安全传输技术（SSL），减少cookie被破解的可能性
3. 只在cookie中存放不敏感数据，即使被盗也不会有重大损失
4. 控制cookie的生命期，使之不会永远有效，偷盗者很可能拿到一个过期的cookie

缺点：
1. cookie数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被拦截
2. 安全性问题。如果cookie被人拦截，那人就可以取得所有的session信息。即使加密也于事无补，因为拦截者并不需要知道cookie的意义，只需要原样转发cookie就可以达到目的了
3. 有些状态不可能保存在客户端。
```

- `DOM` 操作----怎样添加、移除、移动、复制、创建和查找节点

```
1.创建新节点
createDocumentFragment()   // 创建一个DOM片段
createElement()            // 创建一个具体的元素
createTextNode()           // 创建一个文本节点

2.添加、移除、替换、插入
appendChild()
removeChild()
replaceChild()
insertBefore()

3.查找
getElementsByTagName()
getElementsByName()
getElementById()
```

- `null` 和 `undefined` 的区别？

```
null是一个表示‘无’的对象，转为数值时为0
undefined是一个表示‘无’的原始值，转为数值时为NaN

当声明的变量还未被初始化时，变量的默认值为undefined
null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象

undefined表示“缺少值”，就是此处应该有一个值，但是还没有定义：
1. 变量被声明了，但没有赋值时，等于undefined
2. 调用函数时，应该提供的参数没有提供，该参数等于undefined
3. 对象没有赋值的属性，该属性的值为undefined
4. 函数没有返回值时，默认返回undefined

null表示“没有对象”，即该处不应该有值：
1. 作为函数的参数，表示该函数的参数不是对象
2. 作为对象原型链的终点
```

- `new` 操作符具体干了什么

```
1. 创建一个空对象，并且this变量引用该对象，同时还继承了该函数的原型
2. 属性和方法被加入到this引用的对象中
3. 新创建的对象由this所引用，并且最后隐式返回this
```

- `JS` 延迟加载的方式有哪些？

```
1. defer和async
2. 动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
3. 按需异步载入js
```

- 如何判断当前脚本运行再浏览器还是 `Node` 环境中？

```
通过判断全局对象是否为window，如果不为window，当前脚本没有运行在浏览器中。在Node中的全局变量是global，浏览器 全局变量是window，可以通过该全局变量是否定义来判断宿主环境。
```

- 什么是 `"use strict"`；使用它的好处和坏处分别是什么？

```
ECMAScipt 5添加了第二种运行模式：“严格模式”(strict mode)。这种模式使得JavaScript在更严格的条件下运行

设立“严格模式”的目的，主要以下几个：
1. 消除JavaScript语法的一些不合理、不严谨之处，减少一些怪异行为；
2. 消除代码运行的一些不安全指出，保证代码运行的安全；
3. 提高编译器效率，增加运行速度；
4. 为未来新版本的JavaScript做好铺垫

缺点：现在网站的JS都会进行压缩，一些文件用了严格模式，而另一些没有。这时本来是严格模式的文件，被Merge后，就串到文件中间，不仅没有指示严格模式，反而在压缩后浪费了字节。
```

- `ajax` 原理

1. 创建XMLHttpRequest对象，也就是创建一个异步调用对象
2. 创建一个新的HTTP请求，并指定该HTTP请求的方法、URL及验证信息
3. 设置响应HTTP请求状态变化的函数
4. 发送HTTP请求
5. 获取异步调用返回的函数
6. 使用JS和DOM实现局部刷新

- `ajax` 有那些优缺点?
  - 优点：
    - 通过异步模式，提升了用户体验.
    - 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用.
    - `Ajax` 在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载。
    - `Ajax` 可以实现动态不刷新（局部刷新）
  - 缺点：
    - 安全问题 `AJAX` 暴露了与服务器交互的细节。
    - 对搜索引擎的支持比较弱。
    - 不容易调试。

```js
// 1. 创建连接
var xhr = null;
xhr = new XMLHttpRequest();
// 2. 连接服务器
xhr.open('get', url, true);
// 3. 发送请求
xhr.send(null);
// 4. 接受请求
xhr.onreadystatechange = function() {
  if (xhr.readyState == 4) {
    if (xhr.status == 200) {
      success(xhr.responseText);
    } else {
      // fail
      fail && fail(xhr.status);
    }
  }
};
```