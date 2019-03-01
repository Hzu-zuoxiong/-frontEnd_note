# `XSS` 与 `CSRF`

## `XSS`

> 跨网站指令码（英语：`Cross-site scripting`，通常简称为：`XSS`）是一种网站应用程式的安全漏洞攻击，是代码注入的一种。它允许恶意使用者将程式码注入到网页上，其他使用者在观看网页时就会受到影响。这类攻击通常包含了 `HTML` 以及使用者端脚本语言。

### 攻击方式

`XSS` 通过修改 `HTML` 节点或者执行 `JS` 代码来攻击网站。

例如通过 `URL` 获取某些参数

```html
<!-- http://www.domain.com?name=<script>alert(1)</script> -->
<div>{{name}}</div>
```

上述 `URL` 输入可能会将 `HTML` 改为 `<div><script>alert(1)</script></div>` ，这样页面中就凭空多了一段可执行脚本。也有另一种场景，比如写了一篇包含攻击代码 `<script>alert(1)</script>` 的文章，那么可能浏览文章的用户都会被攻击到。

### 防御措施

最普遍的做法是转义输入输出的内容，对于引号，尖括号，斜杠进行转义

```js
function escape(str) {
  str = str.replace(/&/g, '&amp;');
  str = str.replace(/</g, '&lt;');
  str = str.replace(/>/g, '&gt;');
  str = str.replace(/"/g, '&quto;');
  str = str.replace(/'/g, '&#39;');
  str = str.replace(/`/g, '&#96;');
  str = str.replace(/\//g, '&#x2F;');
  return str;
}

// -> &lt;script&gt;alert(1)&lt;&#x2F;script&gt;
escape('<script>alert(1)</script>');
```

### `CSP`

> 内容安全策略 (`CSP`) 是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本 (`XSS`) 和数据注入攻击等。无论是数据盗取、网站内容污染还是散发恶意软件，这些攻击都是主要的手段。

我们可以通过 `CSP` 来尽量减少 `XSS` 攻击。`CSP` 本质上也是建立白名单，规定了浏览器只能够执行特定来源的代码。通常可以通过 `HTTP Header` 中的 `Content-Security-Policy` 来开启 `CSP`。

```js
// 只允许加载本站资源
Content-Security-Policy: default-src ‘self’

// 只允许加载 HTTPS 协议图片
Content-Security-Policy: img-src https://*

// 允许加载任何来源框架
Content-Security-Policy: child-src 'none'
```

## `CSRF`

> 跨站请求伪造（英语：`Cross-site request forgery`），也被称为 `one-click attack` 或者 `session riding`，通常缩写为 `CSRF` 或者 `XSRF`， 是一种挟制用户在当前已登录的 `Web` 应用程序上执行非本意的操作的攻击方法。跟跨网站指令码（`XSS`）相比，`XSS` 利用的是用户对指定网站的信任，`CSRF` 利用的是网站对用户网页浏览器的信任。

简单点说，CSRF 就是利用用户的登录态发起恶意请求。

### 攻击方式

假设网站中有一个通过 `Get` 请求提交用户评论的接口，那么攻击者就可以在钓鱼网站中加入一个图片，图片的地址就是评论接口。

```html
<img src="http://www.domain.com/xxx?comment='attack'" />
```

如果接口是 Post 提交的，就相对麻烦点，需要用表单来提交接口

```html
<form action="http://www.domain.com/xxx" id="CSRF" method="post">
  <input name="comment" value="attack" type="hidden" />
</form>
```

### 防御措施

防范 `CSRF` 可以遵循以下几种规则：

1. `Get` 请求不对数据进行修改
2. 不让第三方网站访问到用户 `Cookie`
3. 阻止第三方网站请求接口
4. 请求时附带验证信息，比如验证码或者 `token`

#### `SameSite`

可以对 `Cookie` 设置 `SameSite` 属性。该属性设置 `Cookie` 不随着跨域请求发送，该属性可以很大程度减少 `CSRF` 的攻击，但是该属性目前并不是所有浏览器都兼容。

#### `验证 Referer`

对于需要防范 `CSRF` 的请求，我们可以通过验证 `Referer` 来判断该请求是否为第三方网站发起的。

#### `Token`

服务器下发一个随机 `Token`（算法不能复杂），每次发起请求时将 `Token` 携带上，服务器验证 `Token` 是否有效。

参考链接： [InterviewMap](https://yuchengkai.cn)
