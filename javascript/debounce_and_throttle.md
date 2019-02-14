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

* **不使用防抖，数字会疯狂地增长。**

* 使用防抖（只执行最后一次）

```js
function debounce(fn, delay) {
    let timer;
    return (...args) => {
        clearTimeout(timer);
        timer = setTimeout(() => {
            fn.apply(this, args);
        }, delay);
    }
}
doc.onmousemove = debounce(todo, 1000);
```

* 使用防抖（只开头执行）

```js
function debounce(fn, delay, isImmediate) {
	let timer;
	return (...args) => {
		clearTimeout(timer);
		if(isImmediate) {
			var isTrigger = !timer;
			timer = setTimeout(() => {
				timer = null;
			}, delay);
			isTrigger && fn.apply(this, args);
		} else {
			timer = setTimeout(() => {
				fn.apply(this,args);
			}, delay);
		}
	}
}
doc.onmousemove = debounce(todo, 1000, true);
```

## 节流

**节流的目的是保证某个操作在一个时间段内只执行一次**。

* 使用节流（利用时间戳，开始立即执行一次，最后不执行）

```js
function throttle(fn, delay) {
	let initTime = 0;
	return (...args) => {
		let now = +new Date();
		if(now - initTime > delay) {
			fn.apply(this, args);
			initTime = now;
		}
	}
}
doc.onmousemove = throttle(todo, 1000);
```

* 使用节流（利用定时器，开始不执行，最后执行一次）

```js
function throttle(fn, delay) {
	let timer = 0;
	return (...args) => {
		if(!timer) {
			timer = setTimeout(() => {
				timer = null;
				fn.apply(this, args);
			}, delay);
		}
	}
}
doc.onmousemove = throttle(todo, 1000);
```

参考链接：[https://juejin.im/post/5b45fa596fb9a04fad3a0268](https://juejin.im/post/5b45fa596fb9a04fad3a0268)

