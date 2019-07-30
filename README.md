# HTTP
超文本传输协议，无状态协议，客户端发起requests——服务端做出responses  
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

```
* HTTP协议版本号  
* 状态码：告知请求执行成功或失败，以及失败原因  
* 状态信息  
* 可选项：响应报文中更常见的包含获取的资源body 
