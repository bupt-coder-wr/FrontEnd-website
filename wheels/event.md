### event

event 类，订阅发布模式

```javascript
/**
  - ES6 创建类
  - 自定义事件机制
  - on 添加事件监听
  - off 取消事件监听
  - once 事件只执行一次
  - trigger 执行事件
 */

class Event {
  constrctor() {
    /**
     * {
     *  event1: [cb1, cb2, cb3],
     *  event2: [cb4, cb5, cb6]
     * }
     */
    this.events = {}
  }

  on(event, cb) {
    if (!this.events[event]) {
      this.events[event] = []
    }
    if (!this.events[event].includes(cb)) {
      this.events[event].push(cb)
    }
    return this
  }

  off(event, cb) {
    if (!this.events[event]) {
      return this
    }
    if (!cb) {
      this.events[event] = null
    } else {
      this.events[event] = this.events[event].filter(item => item !== cb)
    }
    return this
  }

  once(event, cb) {
    const func = (...args) => {
      cb.apply(this, args)
      this.off(event, func)
    }
    this.on(event, func)
    return this
  }

  emit(event, ...args) {
    const cbs = this.events[event]
    if (!cbs) {
      throw TypeError(`${event} event is not registered`)
    }
    cbs.forEach(fn => fn.apply(this, args))
    return this
  }
}
```
