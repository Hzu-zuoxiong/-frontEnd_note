# 变量提升

JavaScript中，函数及变量的声明都将被提升到函数的最顶部。

**实例1：**

```js
console.log(a); // 输出结果 undefined
var a=10;
```

**实例2：**

```js
foo();  // 输出：aaa
function foo(){
    console.log("aaa");
}
```

原理：函数声明提升（函数声明提升直接把整个函数提到执行环境的最顶端），相当于：

```js
function foo(){
    console.log("aaa");
}
foo();
```

**实例3：**

```js
foo();  // foo is not a function,原因：进行了变量提升，js遇到foo()时默认当作函数来解析
var foo = function(){
    console.log("aaa");
}
```

**实例4：**

```js
console.log(foo);
var foo=100;
console.log(foo);
function foo(){
    console.log(10);
}
console.log(foo);
// 第一句console.log(foo);
// f foo(){
//        console.log(10);
//    }
// 第二三句console.log(foo);
// 100
```

原理：函数提升在变量提升上面。相当于：

```js
function foo(){
        console.log(10);
    }
var foo;
console.log(foo);
foo=100;
console.log(foo);
console.log(foo);
```



