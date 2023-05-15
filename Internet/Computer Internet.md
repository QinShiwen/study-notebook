# 计算机网络学习笔记

计算机网络五层模型：应用层 / 传输层 / 网络层 / 数据链路层 / 物理层

### 初识

用户在浏览器中做输入目的URL，浏览器进程先处理用户输入信息，在浏览器内核种向服务器发出请求，获取服务器返回的请求后渲染最后完成页面加载。

## 应用层

### HTTP

Hyper Text Transfer Protocol，超文本传输协议。定义了客户端和服务器之间交换报文的格式和方式。其分为**请求**和**响应**两部分，是简单可扩展的，是无状态协议。

HTTP 有两种连接模式，一种是持续连接，一种非持续连接。

- 非持续连接指的是服务器必须为每一个请求的对象建立和维护一个全新的连接。

- 持续连接下，TCP 连接默认不关闭，可以被多个请求复用。

- 采用持续连接的好处是可以避免每次建立 TCP 连接三次握手时所花费的时间。

- 在 HTTP1.0 以前使用的非持续的连接，但是可以在请求时，加上 Connection: keep-a live。来要求服务器不要关闭 TCP 连接。HTTP1.1 以后默认采用的是持续的连接。目前对于同一个域，大多数浏览器支持 同时建立 6 个持久连接。



#### 请求

**构成**

- 起始行：请求方式POST/GET，HTTP协议版本
- 请求头：请求头包含了一系列的键值对，用来传递请求的附加信息。常见的请求头字段包括Content-Type、User-Agent、Authorization等。请求头允许客户端向服务器传递额外的信息，以便服务器能够更好地理解和处理请求。
- 请求体：请求体是可选的，通常用于POST、PUT等请求方法中传递请求参数或数据。请求体中可以包含任意类型的数据，如表单数据、JSON数据等。

```http
GET /example/path HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
```

#### 请求方法

- GET：通常用于获取数据。
- POST：将实体提交到指定的服务器
- PUT：请求有效载荷替换目标资源
- DELETE：删除指定资源
- OPTIONS：描述目标资源通信选项
- 安全性：不会修改服务器数据的方法：GET/OPTIONS/HEAD，比较安全。
- 幂等性：同样请求被执行一次与连续多次效果是一样的，服务器状态也是一样的。比如：GET/HEAD/OPTIONS/PUT/DELETE



#### 响应

**构成**

- 起始行：HTTP协议的版本、状态码。
- 响应头：响应头包含了一系列的键值对，用于传递响应的附加信息。常见的响应头字段包括Content-Type、Content-Length、Cache-Control等。响应头提供了关于响应内容和服务器的一些元数据信息。
- 响应体：包含了服务器返回的实际内容。响应体的类型和格式取决于请求的性质和服务器的处理结果。对于HTML页面，响应体是HTML代码；对于图片，响应体是图片的二进制数据；对于JSON数据，响应体是JSON字符串等。

```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1234

<!DOCTYPE html>
<html>
<head>
  <title>Example Page</title>
</head>
<body>
  <h1>Hello, world!</h1>
</body>
</html>
```



#### HTTP状态码

- 1xx（Informational）：信息性状态码，表示请求正在处理或需要进一步操作。
  - 100 Continue：服务器已接收到请求的一部分，客户端应继续发送剩余部分。
  - 101 Switching Protocols：服务器已收到请求，客户端应切换到指定的协议。
- 2xx（Success）：成功状态码，表示请求已成功被接收、理解和处理。
  - 200 OK：请求成功，服务器正常返回请求的内容。
  - 201 Created：请求成功，服务器创建了新资源。
  - 204 No Content：请求成功，服务器没有返回任何内容。
- 3xx（Redirection）：重定向状态码，表示需要进一步操作以完成请求。
  - 301 Moved Permanently：请求的资源已永久移动到新位置。
  - 302 Found：请求的资源暂时移动到新位置。
  - 304 Not Modified：客户端缓存的资源仍然有效，未修改。
- 4xx（Client Error）：客户端错误状态码，表示请求包含语法错误或无法完成请求。
  - 400 Bad Request：请求无效，服务器无法理解。
  - 403 Forbidden：服务器理解请求，但拒绝执行。
  - 404 Not Found：请求的资源不存在。
- 5xx（Server Error）：服务器错误状态码，表示服务器在处理请求时发生错误。
  - 500 Internal Server Error：服务器内部错误，无法完成请求。
  - 503 Service Unavailable：服务器暂时无法处理请求，通常是由于过载或维护。



#### HTTP常见首部字段

1. Content-Type：指定了请求或响应中所携带实体的媒体类型。对于请求，它表示请求体的数据类型；对于响应，它表示响应体的数据类型。常见的值包括 **application/json**、 **text/html**、 **image/jpeg** 等。
2. Content-Length：指定了请求或响应体的长度，以字节为单位。在响应中，它可以帮助客户端正确读取响应体的内容。
3. User-Agent：用于标识发送请求的用户代理（通常是浏览器）的信息。服务器可以使用该信息来**识别客户端类型和版本**，并根据需要进行适配或处理。
4. Accept：指定了客户端能够处理的响应内容类型的优先级顺序。服务器可以根据该首部字段来确定最适合的响应类型返回给客户端。
5. Authorization：用于**在请求中传递身份验证凭据**，通常用于访问受保护的资源。它包含了身份验证的类型和凭据信息，比如用户名和密码或访问令牌。
6. Cookie：在请求中传递已存储在客户端的HTTP Cookie。服务器可以使用Cookie来维护会话状态或记录用户的偏好设置。
7. Cache-Control：用于指定**请求或响应的缓存行为**。它可以控制缓存的存储、过期和重新验证策略，以提高性能并减少网络传输。
8. Location：用于重定向响应，指示客户端应该访问的新位置。通常与状态码3xx（重定向）一起使用。
9. If-Modified-Since：在请求中指定一个日期，用于与服务器上的资源的最后修改日期进行比较。服务器可以根据这个字段判断资源是否已被修改，如果没有修改，可以返回304 Not Modified状态码。
10. Referer：指示请求的来源页面的URL。服务器可以使用该信息来进行引荐统计、防盗链等操作。



### HTTPS

HTTPS（Hypertext Transfer Protocol Secure）是一种通过加密和身份验证保护通信安全的HTTP协议。它使用**TLS**（Transport Layer Security）或其前身SSL（Secure Sockets Layer）协议来对HTTP通信进行加密和认证。



#### TLS握手





### DNS



## 传输层

### TCP



## 网络层

### IP



## 题目汇总



