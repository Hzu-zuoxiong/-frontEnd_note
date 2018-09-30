# Browser

提升页面性能的方法有哪些？

1. 资源压缩合并，减少HTTP请求
2. 非核心代码异步加载-----&gt;异步加载的方式-----&gt;异步加载的区别
   1. 异步加载的方式
      1. 动态脚本加载
      2. defer
      3. async
   2. 异步加载的区别
      1. defer是在HTML解析完之后才会执行，如果是多个，按照加载的顺序依次执行
      2. async是在加载完之后立即执行，如果是多个，执行顺序和加载顺序无关
3. 利用浏览器缓存-----&gt;缓存的分类-----&gt;缓存的原理
4. 使用CDN
5. 预解析DNS  
   1. &lt;meta http-equiv="x-dns-prefetch-control" content="on"&gt;

   1. &lt;link rel="dns-prefetch" href="//hostNameToPrefetch.com"&gt;



