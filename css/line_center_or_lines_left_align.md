# CSS 实现一行居中，多行左对齐

想要的效果：

未知文字长度，当文字长度小于盒子宽度，也就是一行时，文字居中。

当文字长度大于盒子的宽度，会自动换行，成为多行文字，此时文字左对齐。

效果如图：

![](/assets/一行居中，多行左对齐.png)

## 思路：

当文字为一行时，则 p 的宽度小于 div 的宽度，p 标签居中显示在盒子内，文字也就居中；

当文字大于一行时，p 的宽度和 div 的宽度一样，文字显示为左对齐。

### display:inline-block

display: inline-block 使 p 的宽度根据文字的宽度进行伸缩。

```css
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>一行居中，多行左对齐</title>
	<style>
	.content {
		float: left;
		margin-left: 5px;
		width: 200px;
		height: 100px;
		border: 1px solid red;
		text-align: center;
	}

	.content > p {
		display: inline-block;
		text-align: left;
	}
	</style>
</head>
<body>
	<div class="content">
		<p>我只有一行</p>
	</div>

	<div class="content">
		<p>我是多行，左对齐的。我是多行，左对齐的。</p>
	</div>
</body>
</html>
```

### display:table

利用未知宽度的 table 可以左右对齐。

```css
.content {
  float: left;
  margin-left: 5px;
  width: 200px;
  height: 100px;
  border: 1px solid red;
}

.content > p {
  display: table;
  margin: 0 auto;
}
```

### 利用图层遮盖

核心是利用绝对定位和 overflow:hidden 的方式，将第一行文字转为居中显示，剩下的行数依旧是左对齐。

```css
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>RunJS</title>
        <style>
        .content{
            position: relative;
            float: left;
            width:140px;
            height:60px;
            margin-right: 5px;
            border: 1px solid red;
        }
        .content .shadow{
            position: absolute;
            width: 100%;
            height: 20px;
            top: 0;
            left: 0;
            overflow: hidden;
            text-align: center;
            background: #fff;
        }
        </style>
    </head>
    <body>
    	<div class="content">
    		<p class="title">短标题</p>
    		<p class="shadow">短标题</p>
    	</div>
    	<div class="content">
    		<p class="title">长标题就是长标题很长的长标题</p>
    		<p class="shadow">长标题就是长标题很长的长标题</p>
    	</div>
    </body>
</html>
```
