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
每个对象都会在其内部初始化一个属性，就是prototype（原型），当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么就会去
prototype里找这个属性，这个prototype又有自己的prototype，于是这样一直寻找下去。
特点：
JavaScript对象是通过引用来传递的，们创建每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，对应的对象也会继承这一改变。
```



