# 防抖与节流

## 防抖

在很多场景会频繁触发事件或频繁向后台发送请求，从而引发性能问题甚至导致浏览器奔溃。**防抖要做的就是短时间内触发同一个事件，只执行最后一次，或者只在开始时执行，中间不执行**。

```markdown
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
        #test {
            width: 800px;
            height: 500px;
            line-height: 500px;
            text-align: center;
            background-color: #0000ffa6;
            font-size: 48px;
            color: white;
        }
    </style>
</head>
<body>
    <div id="test"></div>
    <script type="text/javascript">
        var doc = document.getElementById('test');
        var count = 1;
        function todo() {
            doc.innerHTML = count++;
        }
        doc.onmousemove = todo;
    </script>
</body>
</html>
```

- **不使用防抖，数字会疯狂地增长。**

- 使用防抖（只能在最后执行）

```js
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
doc.onmousemove = debounce(todo, 1000);
```

- 使用防抖（可控制开头或结尾执行）

```js
/**
 * 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
function debounce(func, wait = 50, immediate = true) {
  let timer, context, args;
  // 延迟执行函数
  const later = () =>
    setTimeout(() => {
      // 延迟函数执行完毕，清空缓存的定时器序号
      timer = null;
      // 延迟执行的情况下，函数会在延迟函数中执行
      // 使用到之前缓存的参数和上下文
      if (!immediate) {
        func.apply(context, args);
        context = args = null;
      }
    }, wait);

  // 这里返回的函数是每次实际调用的函数
  return function(...params) {
    // 如果没有创建延迟执行函数（later），就创建一个
    if (!timer) {
      timer = later();
      // 如果是立即执行，调用函数
      // 否则缓存参数和调用上下文
      if (immediate) {
        func.apply(this, params);
      } else {
        context = this;
        args = params;
      }
    } else {
      clearTimeout(timer);
      timer = later();
    }
  };
}
doc.onmousemove = debounce(todo, 1000, true);
```

## 节流

**节流的目的是保证某个操作在一个时间段内只执行一次**。

- 使用节流（利用时间戳，开始立即执行一次，最后不执行）

```js
function throttle(fn, delay) {
  let initTime = 0;
  return (...args) => {
    let now = +new Date();
    if (now - initTime > delay) {
      fn.apply(this, args);
      initTime = now;
    }
  };
}
doc.onmousemove = throttle(todo, 1000);
```

- 使用节流（利用定时器，开始不执行，最后执行一次）

```js
function throttle(fn, delay) {
  let timer = 0;
  return (...args) => {
    if (!timer) {
      timer = setTimeout(() => {
        timer = null;
        fn.apply(this, args);
      }, delay);
    }
  };
}
doc.onmousemove = throttle(todo, 1000);
```

## 总结

&emsp;&emsp;防抖和节流的作用都是防止函数多次调用。区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于 `wait`，防抖的情况下只会调用一次，而节流的 情况会每隔一定时间（参数 `wait`）调用函数。

参考链接：[https://juejin.im/post/5b45fa596fb9a04fad3a0268](https://juejin.im/post/5b45fa596fb9a04fad3a0268)
