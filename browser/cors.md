# CORS

## 简介

---

CORS 是一个 W3C 标准，全称“跨域资源共享”（Cross-origin resource sharing）。它允许浏览器向跨源服务器发出 XMLHttpRequest 请求，从而克服了 AJAX 只能同源使用的限制。CORS 需要浏览器和服务器同时支持。整个 CORS 通信过程，都是浏览器自动完成，不需要用户参与。

## 两种请求

---

浏览器将 CORS 请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。

只要同时满足以下两大条件，就属于简单请求。

> （1\) 请求方法是以下三种方法之一：
>
> - HEAD
> - GET
> - POST
>
> （2）HTTP 的头信息不超出以下几种字段：
>
> - Accept
> - Accept-Language
> - Content-Language
> - Last-Event-ID
> - Content-Type：只限于三个值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`

凡是不同时满足上面两个条件，就属于非简单请求。浏览器对这两种请求的处理，是不一样的。
