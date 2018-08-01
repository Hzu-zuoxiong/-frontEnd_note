# Module

## 简介

在ES6之前，社区制定了一些模块加载方案，最要有`CommonJS`和`AMD`两种。前者用于服务器，后者用于浏览器。ES6实现了模块功能，而且实现得相当简单。ES6模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。`CommonJS`和`AMD`模块，都只能在允许时确定这些东西。

```js
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码实质是加载整个`fs`模块，生成`_fs`对象，再从这个对象读取3个方法。这种加载称为”**运行时加载**“，只有允许时才能得到这个对象，所以完全没办法在编译时做”静态优化“。

```js
// ES6模块
import { stat, exists, readFile } from 'fs';
```

上面代码实质是从`fs`模块加载3个方法，其他方法不加载。这种加载称为”**编译时加载**“或者**静态加载**，即ES6可以在编译时就完成模块加载，效率比`CommonJS`高。ES6模块是编译时加载，使得静态分析称为可能。**ES6模块之中，顶层的`this`指向`undefined`**。

## 严格模式

ES6模块**自动采用严格模式**，不管有没有在模块头部加上`"use strict";`。

严格模式有以下限制：

* 变量必须声明后再使用
* 函数的参数不能有同名属性
* 不能使用`with`语句
* 不能对只读属性赋值
* 不能使用前缀0表示八进制数
* 不能删除不可删除的属性
* 不能删除变量`delete prop`，只能删除属性`delete global[prop]`
* `eval`不会在它的外层作用域引入变量
* `eval`和`arguments`不能被重新赋值
* `arguments`不会自动反映函数参数的变化
* 不能使用`arguments.callee`
* 不能使用`arguments.caller`
* 禁止`this`指向全局对象
* 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
* 增加了保留字（比如`protected`、`static`和`interface`）

## export命令

`export`命令用于规定模块的对外接口，`import`命令用于输入其他模块提供的功能。

一个模块就是一个独立的文件，该文件内部的所有变量，外部无法获取。若要读取模块内部的变量，必须使用`export`输出变量。

```js
// 使用export输出三个变量
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

// 另一种写法，优先考虑使用这种写法
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```

`export`命令还可以输出函数或类。通常情况下，输出的变量是原本的名字，但可以使用`as`关键字重命名。

```js
export function multiply(x, y) {
  return x * y;
};

function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

**注：`export`命令规定是对外的接口，必须与模块内部的变量建立一一对应关系。**

```js
// 错误的写法
// 报错
export 1;

// 通过变量m，还是直接输出1
// 报错
var m = 1;
export m;

// 正确的写法，接口名与模块内部变量之间关系一一对应
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```

`export`语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。这一点与`CommonJS`完全不同。`export`可以出现在模块的任意位置，只要处于模块顶层即可。若处于块级作用域内，则会报错。`import`也是如此。因为处于条件代码块之中，就没法阿左静态优化，与设计初衷相违背。

```js
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

## import命令

`import`命令用于加载其他文件，并从中输入变量。如果**多次重复执行一句`import`语句，那么只会执行一次**，而不会多次执行。`import`命令接受一对大括号，里面指定要从其他模块导入的变量名。**大括号里面的变量名，必须与被导入模块对外接口的名称相同**。`import`后面的`from`指定模块文件的位置，可以是相对路径，也可以是绝对路径，`.js`后缀可以省略。如果只是模块名，不带有路径，则需要配置文件，告诉`JavaScript`引擎该模块的位置。若想为输入的变量重新取一个名字，需要使用`as`关键字。`import`命令输入的变量都是只读的，因为它的本质是输入接口。也就是**不允许在加载模块的脚本里面改写接口**。

```js
// 只会加载一次同一模块
import { foo } from 'my_module';
import { bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';

// 使用as关键字重命名
import { lastName as surname } from './profile.js';

// 不允许在加载模块的脚本里面改写接口
import {a} from './xxx.js'
a = {}; // Syntax Error : 'a' is read-only;
```

**`import`命令具有提升效果**，会提升到整个模块的头部，首先执行。因为`import`命令是编译阶段执行的，在代码运行之前。也是因此，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。

```js
foo();
// 提升
import { foo } from 'my_module';

// 报错
import { 'f' + 'oo' } from 'my_module';

// 报错
let module = 'my_module';
import { foo } from module;

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```

**`import`语句会执行所加载的模块**。仅仅执行模块，但是不输入任何值。

```
import 'lodash';
```

## 模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。

```js
// circle.js
export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

// main.js
import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));
```

**注：模块整体加载的对象，应该是可以静态分析的，所以不允许运行时改变**。

```js
import * as circle from './circle';

// 下面两行都是不允许的
circle.foo = 'hello';
circle.area = function () {};
```

## export default命令

使用`import`命令，用户必须知道所要加载的变量名或函数名，否则无法加载。而`export default`命令可以为模块指定默认输出。其他模块加载该模块时，可以为该匿名函数指定任意名字。这时，`import`命令后面不使用大括号。

```js
// export-default.js
export default function () {
  console.log('foo');
}

// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

`export default`命令用在非匿名函数前也是可以的。

```js
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;
```

显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。所以，`import`命令后面才不用加大括号。本质上，`export default`就是输出一个`default`的变量或方法，然后允许你为它取任意名字。也因此，它后面不能跟变量声明语句。因为`export default`命令的本质是将后面的值，赋给`default`变量，所以可以直接将一个值写在`export default`之后。

```js
// 正确
export var a = 1;

// 正确
var a = 1;
export default a;
export default 42;

// 错误
export default var a = 1;
```

`export default`也可以用来输出类。

```
// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();
```

## export 与 import 的复合写法





