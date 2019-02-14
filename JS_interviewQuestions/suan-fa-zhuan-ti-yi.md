# JS常考算法

* 编写一个times函数，接收2个参数，一个字符串类型str（如"abc"），一个Number类型num（如3），返回一个字符串（这里是"abcabcabc"）。

```js
function times(str,n){
  // join()函数用来将数组转化成字符串，每一项用str连接
  return new Array(n+1).join(str)
}
```

* 编写一个函数判断参数是否是数组类型，是返回true;

```js
function isArray(arr) {
    return Object.prototype.toString.call(arr) === '[Object Array]';
}
```

* 仿JQuery实现JSONP

```js
var JSONP = function(url, callback) {
    // 随机生成函数名
    var cbName = 'JQuery' + new Date().getTime();

    // 将函数挂载在window对象上
    window[cbName] = function(data) {
        callback(data);

        // 用用数据之后，删除创建的script标签
        document.body.removeChild(script);
    }

    // 拼接url，将callback加到url中
    // 如：http://api.douban.com/v2/movie/in_theaters?callback=jQuery1487687886270
    url += url.indexOf('?') == -1 ? '?' : '&';
    url += 'callback=' + cbName;

    // 创建script标签，并添加属性
    var script = document.createElement('script');
    script.src = url;
    script.type = 'text/javascript';

    // 将标签添到文档中
    document.body.appendChild(script);
}
JSONP('https://api.douban.com/v2/movie/in_theaters', function(data) {
   console.log(data); 
});
```

* 假设有一种无色的特殊颜料，与红色颜料混合后会变紫色，与黄色颜料混合会变为绿色，与红色、黄色颜料一起混合会变为黑色，发生颜色变化需要1小时。现有700瓶特殊颜料，其中一瓶已经变质，不管与什么颜料混合都会变为白色。只有一小时时间, 最少需要多少个调色盘才能找出变质的特殊颜料？

```
答案：10个。
解析：10个调色盘，分别当作10个二进制位。700瓶颜料从1到700编号，写成二进制形式，每一瓶颜料，在其二进制为1的位所对应的调色盘上加入。
一小时后，按照变成白色该调色盘对应位为1的原则，写出一个二进制数就是变质颜料的编号。
```

* 请给Array本地对象增加一个原型方法，它用于删除数组条目中重复的条目\(可能有多个\)，返回值是一个包含被删除的重复条目的新数组。

```js
Array.prototype.distinct = function() {
    var ret = [];
    for (var i = 0; i < this.length; i++)
    {
        for (var j = i+1; j < this.length;) {   
            if (this[i] === this[j]) {
                ret.push(this.splice(j, 1)[0]);
            } else {
                j++;
            }
        }
     }
     return ret;
}
//for test
alert(['a','b','c','d','b','a','e'].distinct());
```

* 请给Array本地对象增加一个原型方法，它用于删除数组条目中重复的条目\(可能有多个\)，返回值是一个不含重复元素的新数组。

```js
Array.prototype.uniq = function() {
     return Array.from(new Set(this));
}
//for test
['a','b','c','d','b','a','e'].uniq();
```

* 写一个traverse函数，输出所有页面宽度和高度大于50像素的节点

```js
function traverse(oNode) {
    var aResult = [];
    oNode = oNode || document.body;
    if(oNode.style) {
        var nWidth = window.parseInt(oNode.style.width, 10) || 0;
        var nHeight = window.parseInt(oNode.style.height, 10) || 0;
        if(nWidth > 50 && nHeight > 50) {
            aResult.push(oNode);
        }
    }
    var aChildNodes = oNode.childNodes;
    if(aChildNodes.length > 0) {
        for(var i = 0; i < aChildNodes.length; i++) {
            var oTmp = aChildNodes[i];
            aResult = aResult.concat(traverse(oTmp));
        }
    }
    return aResult;
}
```

* 编写CSS代码，装饰input，当鼠标指向它时，能够出现下方tooltip。
  注意：tooltip上部的小三角是用CSS生成的，并且需要有和tooptip一样的边框。当改变tooltip本身的边框颜色，它也会随同变化

![](/assets/CSS小三角形.png)

```css
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
        .tooltip {
            position: relative;
            width: 250px;
            border: 1px solid #999;
            margin-top: 15px;
            display: none;
        }

        input:hover+.tooltip {
            display: block;
        }

        .tooltip::before {
            position: absolute;
            content: '';
            width: 0;
            height: 0;
            top: -26px;
            left: 10px;
            border: 13px solid;
            border-color: inherit;
            border-top-color: transparent;
            border-right-color: transparent;
            border-left-color: transparent;
        }

        .tooltip::after {
            position: absolute;
            content: '';
            width: 0;
            height: 0;
            top: -25px;
            left: 10px;
            border: 13px solid;
            border-color: transparent transparent white transparent;
        }
    </style>
</head>
<body>

    <div>
        <input placeholder='请输入文字' />
        <div class='tooltip'>只能够输入英文字母、数字和下划线与中划线。</div>
    </div>
</body>
</html>
```

* 实现一个事件类Event，包含以下功能：绑定事件、解绑事件、派发事件和单次事件

```js
class myEvent {
    constructor() {
        this._cache = {};
    }

    on(type, callback) {
        this._cache[type] = this._cache[type] || [];
        let fns = this._cache[type];
        if(fns.indexOf(callback) === -1) {
            fns.push(callback);
        }
        return this;
    }

    emit(type, data) {
        let fns = this._cache[type];
        if(Array.isArray(fns)) {
            fns.forEach((fn) => {
                fn(data);
            });
        }
        return this;
    }

    off(type, callback) {
        let fns = this._cache[type];
        if(Array.isArray(fns)) {
            // callback存在则删除callback，否则清空
            if(callback) {
                let index = fns.indexOf(callback);
                if(index !== -1) {
                    fns.splice(index, 1);
                }
            } else {
                fns.length = 0;
            }
        }
    }

    once(type, callback) {
        let func = (...args) => {
            callback.apply(this, args);
            this.off(type);
        }
        this.on(type, func);
        return this;
    }
}

const event = new myEvent();
event.on('test', (a) => {console.log(a);});
event.emit('test', 'hello world');                 // hello world
event.once('myfunc', () => console.log('once'));
event.emit('myfunc');                              // once
event.emit('myfunc');                              // 未被触发
event.off('test');                                 // 解绑事件
event.emit('test');                                // 未被触发
```

* 实现对象的深拷贝

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

// 测试
let obj = {
    a: 1,
    b: 'string',
    c: [1,2,3],
    d: {
        b: 1,
        a: [5,'w',{z:5, b:3}],
        z: 'string',
        s: {
            b:2
        }
    }
}
let emptyObj = deepClone(obj);
console.log(emptyObj.d.a[2] === obj.d.a[2]);
```

* \["1", "2", "3"\].map\(parseInt\)输出什么？

```js
// parseInt函数能解析一个字符串，并返回一个整数，需要两个参数(val, radix)，其中radix表示要解析的字符串的基数【该值介于2~36之间，
// 并且字符串中的数字不能大于radix才能正确返回数值结果】，但此处map传了三个参数(element, index, array)，重写parseInt测试规则：
 function parseInt(str, radix) {
     return str+'-'+radix;
 };
 var a=["1", "2", "3"];
 a.map(parseInt);  // ["1-0", "2-1", "3-2"] 不能大于radix
 // 因为没有一进制，二进制没有数字3，故后面两个都是NaN.
 // 所以，最后["1", "2", "3"].map(parseInt)输出：[1, NaN, NaN]
```





