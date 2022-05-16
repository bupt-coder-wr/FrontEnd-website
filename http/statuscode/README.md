### 1.x 系列

> 表示临时响应并需要请求者继续执行操作的状态码。

- 100 `Continue`
- 101 `Switching Protocol`

### 2.x 系列

> 表示成功处理了请求的状态码。

- 200 `OK`
- 201 `Created`
- 202 `Accepted`
- 204 `No Content`

### 3.x 系列

> 表示要完成请求，需要进一步操作。

- 301 `Moved Permanently` 永久重定向
- 302 `Found` 暂时重定向
- 304 `Not Modify` (协商缓存)
- 305 `Use Proxy`

  301 和 302 应用
  301: http->https; 域名更改; 登陆后页面跳转;
  302:

### 4.x 系列

> 客户端错误

- 400 `Bad Request`
- 401 `Unauthorized`
- 403 `Forbiden`
- 404 `Not Found`
- 405 `Method Not Allowed`

  401 和 403 区别
  401: 表示缺失或错误的认证，从性质上来说是临时的东西。
  403: 用户被认证后，但用户没有被授权在特定资源上执行操作，从性质上来说是永久的东西。收到 403 响应表示服务器完成认证过程，但是客户端请求没有权限去访问要求的资源。

### 5.x 系列

> 服务端错误

- 500 `Internal Server Error`
- 501 ` Not Implemented`
- 502 `Bad Gateway`
- 503 `Service Unavailable`
- 504 `Gateway Timeout`
- 505 `HTTP Version not supported`
