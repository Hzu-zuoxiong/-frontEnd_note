# 算法专题一

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



