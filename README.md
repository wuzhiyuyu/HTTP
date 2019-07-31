# HTTP
超文本传输协议，无状态协议，Client——Server结构，客户端发起requests——服务端做出responses  
代理proxy，介于客户端与服务端之间，作用：缓存、过滤、负载均衡、认证、日志记录  
HTTP头部扩展（HTTP headers/HTTP Cookies  
  
### HTTP**控制**  
* 缓存  
* 开放同源限制：只有相同来源的网页才能够获取网站的全部信息  
* 认证  
* 代理和隧道  
* 会话  
  
### HTTP流  
客户端与服务端信息交互过程  
1. 打开TCP连接：发送请求及接受响应  
2. 发送一个HTTP报文  
3. 读取服务端返回报文信息  
4. 关闭连接或者为后续请求重用连接  
  
### HTTP请求报文  
例：
```
GET/HTTP/1.1  
Host:developer.mozilla.org  
Accept-Language:fr  
```
* Method：GET/POST/OPTIONS/HEAD  
* 路径Path  
* HTTP协议版本号  
* 为服务端表达其他信息的可选头部headers  
* 对于一些像POST这样的方法，报文的body就包含了发送的资源，这与响应报文的body类似  
  
### 响应报文  
```
HTTP/1.1 200 OK  
Date:Sat,09 Oct 2010 14:28:02 GMT  
Content-Length:29769  
Content-Type:text/html  
```
* HTTP协议版本号  
* 状态码：告知请求执行成功或失败，以及失败原因  
* 状态信息  
* 可选项：响应报文中更常见的包含获取的资源body 
  
## HTTP缓存  
（私有）浏览器缓存、（公有）代理缓存，常见的HTTP缓存只能存储GET响应。缓存的关键主要包括request method和目标URL  
缓存操作的目标  
* 一个检索请求成功的响应：200  
* 永久重定向：301  
* 错误响应：404  
* 不完全响应：206  
* 除了 GET 请求外，如果匹配到作为一个已被定义的cache键名的响应  
  
### 缓存控制  
#### Cache-control头  
请求和响应都支持
**禁止进行缓存**  
```
Cache-Control: no-store  
```
**强制确认缓存**  
```
Cache-Control: no-cache
```
**私有缓存和公共缓存**
```
Cache-Control: private  
Cache-Control: public  
```
**缓存过期机制**  
``` 
Cache-Control: max-age=31536000  
```
**缓存验证确认**
```
Cache-Control: must-revalidate  
```
#### Pragma头  
在请求中与Cache-Control: no-cache相同，但在响应头不支持  
  
新鲜度 缓存驱逐  
**缓存寿命**一般包含在max-age/Expires/Date/Last-Modified中，齐、其值=max-age=(date-Last-Modified)/10  
缓存失效时间EexpirationTime = responseTime + freshnessLifetime - currentAge  
  
### 缓存验证  
1. 用户点击刷新按钮时会开始缓存验证  
2. 如果缓存的响应头信息里含有*Cache-control: must-revalidate*，在浏览的过程中也会触发缓存验证  
3. 在浏览器偏好设置里设置Advanced->Cache为强制验证缓存  
  
当缓存的文档过期后，需要进行缓存验证或者重新获取资源。只有在服务器返回强校验器或者弱校验器时才会进行验证  
**ETags**强校验器，如果资源请求的响应头里含有ETag, 客户端可以在后续的请求的头中带上If-None-Match头来验证缓存，若返回200 OK镖师返回正常，304 Not Modified(不返回body)表示浏览器可以使用本地缓存文件，304响应头还可以同时更新缓存文档的过期时间  
  
### Vary头响应  
判断请求新资源还是使用缓存文件，只有当前请求和原始请求头跟缓存响应头里的Vary都匹配，才能使用缓存响应  
例如，用*Vary:User-Agent*区分移动版和桌面客户端  
  
## HTTP Cookies  
* 会话状态管理  
* 个性化设置  
* 浏览器行为跟踪  
### 创建Cookie
#### Set-Cookie响应头部和Cookie请求头部  
服务器使用响应头部向用户代理发送Cookie信息，服务器通过该头部告知客户端保存Cookie信息
```
HTTP/1.0 200 OK  
Content-type: text/html  
Set-Cookie: yummy_cookie=choco             //Set-Cookie: <cookie名>=<cookie值>  
Set-Cookie: tasty_cookie=strawberry  
  
[页面内容]  
```
之后每对服务器发起一次请求，浏览器都会将之前保存的Cookie信息通过Cookie请求头部再发送给给服务器  
```
GET /sample_page.html HTTP/1.1  
Host: www.example.org  
Cookie: yummy_cookie=choco; tasty_cookie=strawberry  
```
**会话期Cookie**  
不需要指定过期时间（Expires）或者有效期（Max-Age），在浏览器关闭时自动删除，但有些浏览器提供会话恢复功能  
**持久性Cookie**  
指定特定的过期时间或有效期，设定的时间只与客户端相关，而不是服务端  
```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;Secure; HttpOnly  
```
**Secure和HttpOnly标记**  
Secure标记的Cookie只应通过HTTPS协议加密过的请求发送给服务端  
HttpOnly标记的只发送给服务端，不会被客户端JS脚本调用  
**Cookie作用域**  
即Cookie应该发送给哪些URL  
*Domain*标识指定哪些主机可以接受Cookie，默认为当前文档的主机（不包含子域名）  
如果设置 Domain=mozilla.org，则Cookie也包含在子域名中（如developer.mozilla.org）  
*Path*指定主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中），以字符 %x2F ("/") 作为路径分隔符，子路径也会被匹配。  
**Doucment.cookies**  
通过Document.cookie可创建新的Cookie，也可通过该属性访问非HttpOnly标记的Cookie  
```
document.cookie = "yummy_cookie=choco";   
document.cookie = "tasty_cookie=strawberry";   
console.log(document.cookie);   
// logs "yummy_cookie=choco; tasty_cookie=strawberry"  
```
  
会话劫持和XSS  
跨站请求伪造（CSRF）  
第三方Cookie（如图片广告）  
禁止追踪Do-Not-Track  
欧盟Cookie指令  
僵尸Cookie（删不掉）
  
### HTTP访问控制(CORS)  






