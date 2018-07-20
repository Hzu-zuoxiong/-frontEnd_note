# 防抖与节流

## 防抖

在很多场景会频繁触发事件或频繁向后台发送请求，从而引发性能问题甚至导致浏览器奔溃。防抖要做的就是短时间内触发同一个事件，只执行最后依次，或者只在开始时执行，中间不执行。

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

* 不使用防抖，数字会疯狂地增长。

![](/assets/防抖1.gif)

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

 

