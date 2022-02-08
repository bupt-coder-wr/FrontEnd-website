### localStorage，sessionStorage，cookie

|              | localStorage | sessionStorage |    cookie    | indexDB |   session    |
| ------------ | :----------: | :------------: | :----------: | :-----: | :----------: |
| 存储大小     |     5MB      |      5MB       |     4KB      |  不限   |     不限     |
| 生命周期     |   手动清除   |    关闭窗口    | 设置生命周期 |  手动   | 函数调用超时 |
| 存储位置     |    客户端    |     客户端     |    客户端    | 客户端  |    服务端    |
| 于服务器通信 |    不通信    |     不通信     |     通信     | 不通信  |              |
| 安全性       |      低      |       低       |      低      |   低    |      高      |

对于 `cookie` 的属性

| 属性            |                         作用                          |
| --------------- | :---------------------------------------------------: |
| Name            |                          key                          |
| Value           |                         value                         |
| Domain          |                      使用的域名                       |
| Path            |                      使用的路径                       |
| Expires/Max-age |                       过期时间                        |
| Size            |                         大小                          |
| HttpOnly        |        不能通过 JS 访问 Cookie，减少 XSS 攻击         |
| Secure          |            只能在协议为 HTTPS 的请求中携带            |
| SameSite        | 规定浏览器不能在跨域请求中携带 Cookie，减少 CSRF 攻击 |
