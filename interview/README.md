### 字节一面

- React 和 Vue 如何选择？
- Vue 中 computed 是如何监听数据变化的？
- React Hook 为什么不能出现在判断语句中？
- 客户端如何与 H5 做交互？
- this 绑定的问题（立即执行函数中的 this）
- TCP 和 UDP 的区别？TCP 是如何保证数据的安全传输的？
- HTTPS 的加密过程？证书的认证过程是怎样的？那个阶段是对称加密的？为什么要分对称和非对称？
- 跨域问题？如何解决跨域？如果我去请求 bytedance.com 的图片会怎样？
- 手动实现 Promise.all

### bilibili 一面

- 为什么做前端？
- http2.0 有什么特点？如何多路复用？
- TCP 七层协议模型？
- Node.js 有了解吗？
- var，let 区别
- for.of.?什么是迭代器？迭代器的特点是什么？
- React fiber 有了解吗？
- 为什么要使用 virtualDom？diff 算法是如何进行的？Vue 的 diff 详细过程？
- 是否自己写过 webpack Plugin？webpack 热更新原理是什么？
- px em rem 的区别？
- 前端性能优化？
- 介绍简历中有亮点的一段项目
- JS 的代码执行顺序
- 杨辉三角的算法

### 滴滴出行 一面

- Vuex 的工作流程
- Get 和 Post 的区别
- 如何进行页面适配？fontSize
- 如何排查线上问题是那个接口挂了
- 浏览器缓存是什么？应该对什么内容进行缓存？
- 如何解决跨域问题？Access-Control-Allow-Origin 字段怎么用？如何携带 coikie？
- Webpack 构建流程是什么？有没有写过 plugin 或 loader？如何在一个项目中输出两块内容？
- Vue 怎么对数据进行监听？直接修改数组项无什么没有进行监听？为什么对 push，pop 等方法进行监听？
- $router 和 $route 有什么区别？路由跳转用那个？
- 有没有使用过$nextTick？触发时机是什么
- History 和 Hash 有什么区别？使用 history，跳转过后刷新页面会出现什么问题？
- 有没有使用过 createElement API？什么情况适合使用？
- 什么是异步组件？什么时候使用异步组件？

### 客户端如何实现与网页通信？

> https://juejin.cn/post/6844903585268891662

使用 JSBridge。JSBridge 主要是**给 Javascript 提供调用 Native 功能的接口**，让混合开发中的前端部分使用客户端的数据信息。

核心是构建 Native 与非 Native 间消息通信的通道，而且是双向通行通道。

JSBridge 实现原理：

Javascript 是运行在 Javascript Context 中。由于这些 Context 于原生运行环境的天然隔离，可以将 Javascript 调用 Native 看作是一次 RPC 调用。把前端看作是 RPC 的前端，把 Native 看作 RPC 的服务端。

JSBridge 的通信原理：

- Javascript 调用 Native

  - 注入 API

    通过 WebView 提供的接口，向 Javascript 的 Context 中注入对象或者方法，让 javascript 调用时，直接执行相应的 Native 代码逻辑，达到 Javascript 调用 Native 的目的

  - 拦截 URL Scheme

- native 调用 Javascript

  Native 调用 Javascript，实际上就是执行拼接 Javascript 字符串，从外部调用 Javascript 中的方法，因此 Javascript 中的方法必须在全局的 window 上。

### React Fiber 有了解吗？

> https://segmentfault.com/a/1190000018250127

React Fiber 主要解决了页面元素很多，且需要频繁刷新的场景下，React15 会出现的掉帧现象。

根本原因：大量的同步计算任务阻塞了浏览器的 UI 渲染。当调用 setState 更新页面的时候，react 会便利所有的节点，计算差异，然后再更新 UI。。如果页面元素过多，计算的过程超过 16ms，就容易出现掉帧。

解决问题的思路：

解决主线程长时间被 JS 运算占用这一问题的基本思路就是将运算切割成多个步骤，分批完成。也就是说在完成一部分任务之后，将控制权交给浏览器，让浏览器先进行页面的渲染。

旧版 React 通过递归的方式进行渲染，使用的是 JS 引擎自身的函数调用栈。他会一直执行到栈空。而 Fiber 实现了自己的组件调用栈，它以链表的形式遍历组件树，可以灵活的暂停，继续和丢弃执行的任务。

主要对 reconcile 层进行变动，取新名字为`Fiber reconcile`，为了区分之前的叫做`stack reconcile`。

Fiber 指的是一种数据结构。`Stack Reconciler` 运作的过程是不能被打断的。而 `Fiber Reconciler` 每执行一段时间，都会将控制权交回给浏览器，可以分段执行。

```javascript
const fiber = {
    stateNode,    // 节点实例
    child,        // 子节点
    sibling,      // 兄弟节点
    return,       // 父节点
}
```

为了达到这种效果，需要有一个调度器来进行任务分配，并且任务具有不同的优先级。高优先级的可以打断低优先级的任务。

Fiber reconcile 执行过程分为两个阶段：

- 阶段一：生产 Fiber 树，得出需要更新的节点信息，这一步可以被打断
- 阶段二：将需要更新的节点一次过批量更新，不能被打断

Fiber Reconciler 在阶段一进行 Diff 计算的时候，会生成一棵 Fiber 树。这棵树是在 Virtual DOM 树的基础上增加额外的信息来生成的，它本质来说是一个链表。

在后续需要 Diff 的时候，会根据已有树和最新 Virtual DOM 的信息，生成一棵新的树。这颗新树每生成一个新的节点，都会将控制权交回给主线程，去检查有没有优先级更高的任务需要执行。如果没有，则继续构建树的过程;如果过程中有优先级更高的任务需要进行，则 Fiber Reconciler 会丢弃正在生成的树，在空闲的时候再重新执行一遍。
