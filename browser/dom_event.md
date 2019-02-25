# DOM 事件机制

## DOM 事件的级别：

- `DOM0：element.onclick = function() { }`;
- `DOM2：element.addEventListener('click', function() { }, false)`;
- `DOM3：element.addEventListener('keyup', function() { }, false)`;

## Event 事件常见应用

```js
// 当前被点击的元素
e.target;
// 阻止默认行为
e.preventDefault();
// 阻止事件冒泡,也可以阻止捕获事件（在自由注册事件的时候）。
e.stopPropagation();
// 同样也能实现阻止事件，但是还能阻止该事件目标执行别的注册事件。
// 按钮绑定了两个click事件 A、B，希望点击 A 之后加上这句话，就可以阻止 B 的执行。
e.stopImmediatePropagation();
```

## 事件代理

如果一个节点中的子节点是动态生成的，那么子节点需要注册事件的话应该注册在父节点上。这样做有其优点：

- 节省内存
- 不需要给子节点注销事件

```html
<ul id="ul">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
</ul>
<script>
  let ul = document.querySelector('##ul');
  ul.addEventListener('click', event => {
    console.log(event.target);
  });
</script>
```

## DOM 事件模型：

- 捕获流程：`window --> document --> html(document.documentElement) --> body --> ... --> 目标元素`
- 冒泡流程：与捕获流程相反

捕获流程代码演示：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Event</title>
  </head>
  <body>
    <div id="ev">
      <style media="screen">
        #ev {
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
      ev.addEventListener(
        'click',
        function(e) {
          console.log('ev captrue');
        },
        true
      );
      // body
      document.body.addEventListener(
        'click',
        function(e) {
          console.log('body captrue');
        },
        true
      );
      // html
      document.documentElement.addEventListener(
        'click',
        function(e) {
          console.log('html captrue');
        },
        true
      );
      // document
      document.addEventListener(
        'click',
        function(e) {
          console.log('document captrue');
        },
        true
      );
      // window
      window.addEventListener(
        'click',
        function(e) {
          console.log('window captrue');
        },
        true
      );
    </script>
  </body>
</html>
```

## 效果：![](/assets/browser/DOMEvent.png)自定义事件：

```js
// 创建自定义事件
var eve = new Event('test');
// 监听自定义事件
ev.addEventListener('test', function() {
  console.log('test dispatch');
});
setTimeout(function() {
  // 自定义事件触发
  ev.dispatchEvent(eve);
}, 1000);
```
