# 错误处理

## 错误监控类

* 前端错误的分类
* 错误的捕获方式
* 上报错误的基本原理

## 错误的捕获方式

1. 即时运行错误的捕获方式
   1. try...catch
   2. window.onerror
2. 资源加载错误
   1. object.onerror
   2. performance.getEntries\(\)
   3. Error事件捕获

延伸：跨域的JS运行错误可以捕获吗，错误提示什么，应该怎么处理？

![](/assets/错误处理.png)

1. 在script标签增加crossorigin属性
2. 设置JS资源响应头Access-Control-Allow-Origin: \*

## 上报错误的基本原理

1. 采用Ajax通信的方式上报
2. 利用Image对象上报



