# 算法专题一

* 编写一个times函数，接收2个参数，一个字符串类型str（如"abc"），一个Number类型num（如3），返回一个字符串（这里是"abcabcabc"）。

```js
function times(str,n){
  // join()函数用来将数组转化成字符串，每一项用str连接
  return new Array(n+1).join(str)
}
```

* 编写一个函数判断参数是否是数组类型，是返回true;

```
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



