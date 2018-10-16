# JavaScript考点

* 介绍JS的基本数据类型

```
undefined、null、boolean、number、string、symbol

JavaScript有两种数据类型：原始数据类型和引用数据类型
存储在栈中：原始数据类型（undefined、null、boolean、number、string）
存储在堆中：引用数据类型（对象、数组和函数）
```

* 介绍JS的内置对象

```
Object是JS中所有对象的父对象
数据封装类对象：Obeject、Array、Boolean、Number、String
其他对象：Function、Arguments、Math、Date、RegExp、Error
```

* JavaScript的基本规范？

```
1.不要在同一行声明多个变量。
2.请使用=== / !== 来比较true/false或者数值
3.使用对象字面量代替new Array这种形式
4.switch语句必须带有default分支
5.函数不应该有时候有返回值，有时候没有返回值
6.for循环必须使用大括号
7.if语句必须使用大括号
8.for in循环中的变量应使用var关键字明确限定作用域，避免作用域污染
```

* JavaScript原型，原型链？有什么特点？

```
每个对象都会在其内部初始化一个属性，就是prototype（原型），当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么
就会去prototype里找这个属性，这个prototype又有自己的prototype，于是这样一直寻找下去。

特点：
JS对象是通过引用来传递的，们创建每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，对应的对象也会继承这一改变。
```

* 谈谈cookie的优缺点

```
优点：极高的扩展性和可用性
1.通过良好的编程，控制保存在cookie中的session对象的大小
2.通过加密和安全传输技术（SSL），减少cookie被破解的可能性
3.只在cookie中存放不敏感数据，即使被盗也不会有重大损失
4.控制cookie的生命期，使之不会永远有效，偷盗者很可能拿到一个过期的cookie

缺点：
1.cookie数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被拦截
2.安全性问题。如果cookie被人拦截，那人就可以取得所有的session信息。即使加密也于事无补，因为拦截者并不需要知道cookie的意义，只
  需要原样转发cookie就可以达到目的了
3.有些状态不可能保存在客户端。
```

* 简单说一下浏览器本地存储是怎样的

```
HTML5的Web Storage包括两种存储方式：sessionStorage和localStorage

sessionStorage：用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也会销毁。
                sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。
localStorage：用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。
```



