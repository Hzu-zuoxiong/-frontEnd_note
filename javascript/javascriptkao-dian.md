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

* 线程与进程的区别？

```
1.一个程序至少有一个进程，一个进程至少有一个线程
2.线程的划分尺度小于进程，使得多线程程序的并发性高
3.另外，进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率
4.线程在执行过程中与进程还是有区别的。每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立运行，
  必须依赖在应用程序中，由应用程序提供多个线程执行控制
5.从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看作多个独立的应用，
  来实现进程的调度和管理以及资源分配。这是进程和线程的重要区别。
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

* `DOM`操作----怎样添加、移除、移动、复制、创建和查找节点

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

* `null`和`undefined`的区别？

```
null是一个表示‘无’的对象，转为数值时为0
undefined是一个表示‘无’的原始值，转为数值时为NaN

当声明的变量还未被初始化时，变量的默认值为undefined
null用来表示尚未存在的对象，常用来表示函数企图返回一个不存在的对象

undefined表示“缺少值”，就是此处应该有一个值，但是还没有定义：
1.变量被声明了，但没有赋值时，等于undefined
2.调用函数时，应该提供的参数没有提供，该参数等于undefined
3.对象没有赋值的属性，该属性的值为undefined
4.函数没有返回值时，默认返回undefined

null表示“没有对象”，即该处不应该有值：
1.作为函数的参数，表示该函数的参数不是对象
2.作为对象原型链的终点
```

* `new`操作符具体干了什么

```
1.创建一个空对象，并且this变量引用该对象，同时还继承了该函数的原型
2.属性和方法被加入到this引用的对象中
3.新创建的对象由this所引用，并且最后隐式返回this
```

* `JS`延迟加载的方式有哪些？

```
1.defer和async
2.动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
3.按需异步载入js
```

* 如何解决跨域问题？

```
1.jsonp（jsonp的原理是动态插入script标签）
2.document.domain + iframe
3.window.name
4.postMessage
5.跨域资源共享（CORS）
```

* 哪些操作会造成内存泄漏？

```
内存泄漏指任何对象不再需要之后它仍然存在。
垃圾挥手器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为0（没有其他对象引用过该对象），或该对象的
唯一引用是循环的，那么该对象的内存即可回收。

1.setTimeout的第一参数使用字符串而非函数的话，会引发内存泄漏
2.闭包
3.控制台日志
4.循环（在两个对象彼此引用且彼此保存时，会产生一个循环）
```



