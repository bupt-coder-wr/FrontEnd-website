## 强缓存

实现强缓存的控制字段：`Expires` 和 `Cache-Control`

请求头：`Cache-Control`

响应头：`Expires`

```javascript
Expires: Wed, 22 Oct 2018 08:41:00 GMT
```

`Expires` 是 HTTP/1.0 的产物。状态码 `200`。如果用户手动更改系统时间，会造成缓存强制过期。

```javascript
Cache-control: max-age=30
```

`Cache-control` 出现于 HTTP/1.1。优先级高于 Expires。
`Cache-control` 的属性

| 属性     |                          含义                           |
| -------- | :-----------------------------------------------------: |
| public   |            所有内容均可缓存（客户端和代理）             |
| private  |                只有客户端可缓存（默认）                 |
| max-age  |                      最大生存周期                       |
| no-store |                     所有均不可缓存                      |
| no-cache | 客户端可缓存，但每次都要重新验证 效果等同于 max-age = 0 |

## 协商缓存

状态码：304

实现协商缓存的控制字段：`Last-Modified`，`If-Modify-Since`，`ETag`，`If-None-Match`

请求头：`If-Modify-Since`，`If-None-Match`

响应头：`Last-Modified`，`ETag`

`Last-Modified` 和 `If-Modify-Since`

`Last-Modified` 表示文件最后修改日期，`If-Modify-Since` 会将 `Last-Modified` 的值发送给服务器，询问是否有更新，有更新就会将资源返回。
但是如果在本地打开缓存资源，会造成 `Last-Modified` 被修改，所以在 `HTTP/1.1` 中出现了 `ETag`

`ETag` 和 `If-None-Match`

`ETag`类似于文件指纹，`If-None-Match`会将当前 `ETag` 发送给服务器，询问该资源 `ETag` 是否变动，有变动的话就将资源发送回来。`ETag` 优先级比 `Last-Modified` 高

> 内容参考： https://juejin.cn/post/6844903593275817998
