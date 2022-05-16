## 跨域

> https://juejin.cn/post/6999977495550394404

1. 什么是跨域

   > 受同源策略的影响，浏览器不允许浏览器加载非同源的资源，阻止了一个域的 JavaScript 脚本和另一个域的内容进行交互
   > 是浏览器的一种自我保护机制

2. 同源策略

   > 协议，域名，端口号，三者都相同即为同源
   > 有一个不同就是非同源

3. 跨域的判定流程

- 网页上的代码，向服务器发起 XMLHttpRequest
- 如果为**简单请求**，浏览器直接向服务器发送请求，服务器正常返回，浏览器拿到后判断资源是否可以跨域
  - 如果跨域：浏览器报错，阻止 JS 执行
  - 如果没有跨域：JS 正常响应，浏览器继续执行后续业务
- 如果为**复杂请求**，浏览器回预先发送 `OPTIONS` 请求，然后根据浏览器的反应，作出反应
  - 如果跨域：实际请求**不会**发出
  - 如果没有跨域：实际请求会进行发送，进行后续业务处理

**注意： ⚠️**跨域机制主要是阻止数据的跨域获取，不是阻止请求的发送。

4. 简单请求和非简单请求

- 简单请求（simple request）
  - 同时满足以下两大条件,就属于简单请求
    - 使用请求方法
      - get
      - post
      - head
    - Content-type 值仅限于以下三个之一
      - application/x-www-form-urlencoded
      - text/plain
      - multipart/form-data
- 非简单请求（not-so-simple request）
  - 不满足以上的两大条件就为非简单请求,可以自定义 content-type

5. **跨域的解决方案**

- 设置 document.domain 解决无法读取非同源网页的 Cookie 问题
  因为浏览器通过 document.domain 来检查两个页面是否同源，因此只要通过设置相同的 document.domain，两个页面就可以共享 Cookie（此方案仅限主域相同，子域不同的跨域应用场景。）

  ```javascript
  // 两个页面都设置
  document.domain = 'test.com'
  ```

- JSONP

  JSONP 是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，兼容性好（兼容低版本 IE），缺点是只支持 get 请求，不支持 post 请求。

  核心思想：网页通过添加一个`<script>`元素，向服务器请求 JSON 数据，服务器收到请求后，将数据放在一个指定名字的回调函数的参数位置传回来。

  1. 原生实现

  ```javascript
  // 向服务器test.com发出请求，该请求的查询字符串有一个callback参数，用来指定回调函数的名字
  <script src="http://test.com/data.php?callback=dosomething"></script>

  // 处理服务器返回回调函数的数据
  <script>
  function dosomething(res){
      // 处理获得的数据
      console.log(res.data)
  }
  </script>
  ```

  2. vue 实现

  ```javascript
  this.$http
    .jsonp('http://www.domain2.com:8080/login', {
      params: {},
      jsonp: 'handleCallback',
    })
    .then(res => {
      console.log(res)
    })
  ```

- CORS
  CORS 是跨域资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，属于跨源 AJAX 请求的根本解决方法。

  1. 普通跨域请求：只需服务器端设置 `Access-Control-Allow-Origin`
  2. 带 cookie 跨域请求：前后端都需要进行设置
     【前端设置】根据 xhr.withCredentials 字段判断是否带有 cookie

     1. 原生 AJAX 解决：

     ```javascript
     const xhr = new XMLHttpRequest()
     xhr.withCredentials = true
     ```

     2. axios 解决：

     ```javascript
     axios.defaults.withCredentials = true
     ```

     【服务端设置】
     服务器端对于 CORS 的支持，主要是通过设置 Access-Control-Allow-Origin 来进行的。如果浏览器检测到相应的设置，就可以允许 Ajax 进行跨域的访问。

- nginx 反向代理
  设置 proxy_pass
