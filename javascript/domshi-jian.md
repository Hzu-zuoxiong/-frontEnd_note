# DOM事件

## DOM事件的级别：

* DOM0：element.onclick = function\(\) { };
* DOM2：element.addEventListener\('click', function\(\) { }, false\);
* DOM3：element.addEventListener\('keyup', function\(\) { }, false\);

## Event事件常见应用

```js
// 当前被点击的元素 
e.target
// 阻止默认行为
e.preventDefault() 
// 阻止事件冒泡
e.stopPropagation() 
//按钮绑定了两个click事件A、B，希望点击A之后加上这句话，就可以阻止B的执行。
e.stopImmediatePropagation()
```

## DOM事件模型：

* 捕获流程：window --&gt; document --&gt; html\(document.documentElement\) --&gt; body --&gt;  ......  --&gt; 目标元素
* 冒泡流程：与捕获流程相反

捕获流程代码演示：

```js
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Event</title>
  </head>
  <body>
      <div id="ev">
        <style media="screen">
          #ev{
            width: 1000px;
            height: 500px;
            background: red;
            color: #fff;
            text-align: center;
            line-height: 500px;
            font-size: 72px;
          }
        </style>
        目标元素
      </div>
      <script type="text/javascript">
        var ev = document.getElementById('ev');

        // 目标元素
        ev.addEventListener('click', function (e) {
            console.log('ev captrue');
        }, true);
        // body
        document.body.addEventListener('click', function (e) {
            console.log('body captrue');
        }, true);
        // html
        document.documentElement.addEventListener('click', function (e) {
            console.log('html captrue');
        }, true);
        // document
        document.addEventListener('click', function (e) {
            console.log('document captrue');
        }, true);
        // window
        window.addEventListener('click', function (e) {
            console.log('window captrue');
        }, true);
      </script>
  </body>
</html>
```

## 效果：![](/assets/DOM事件.png)自定义事件：

```js
// 创建自定义事件
var eve = new Event('test');
// 监听自定义事件
ev.addEventListener('test', function () {
    console.log('test dispatch');
});
setTimeout(function () {
    // 自定义事件触发
    ev.dispatchEvent(eve);
}, 1000);
```



