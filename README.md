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
