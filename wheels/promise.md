### Promise

```javascript
class MyPromise {
  constructor(executor) {
    this.status = 'pending'
    this.value = undefined
    this.reason = undefined
    // 成功的回调
    this.onResolvedCallbacks = []
    // 失败的回调
    this.onRejectedCallbacks = []
    const resolve = data => {
      if (this.status === 'pending') {
        this.value = data
        this.status = 'resolved'
        this.onResolvedCallbacks.forEach(cb => cb())
      }
    }
    const reject = reason => {
      if (this.status === 'pending') {
        this.reason = reason
        this.status = 'rejected'
        this.onRejectedCallbacks.forEach(cb => cb())
      }
    }
    // 执行时可能发生异常
    try {
      executor(resolve, reject)
    } catch (e) {
      reject(e)
    }
  }
  then(onFulfilled, onRejected) {
    // 如果then方法的参数不为函数，直接跳过
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : () => {}
    onRejected = typeof onRejected === 'function' ? onRejected : () => {}
    // then方法执行后返回新的Promise
    let promise = new MyPromise((resolve, reject) => {
      if (this.status === 'resolved') {
        setTimeOut(() => {
          onFulfilled(this.value)
        })
      }
      if (this.status === 'rejected') {
        setTimeOut(() => {
          onRejected(this.reason)
        })
      }
      if (this.status === 'pending') {
        this.onResolvedCallbacks.push(() => {
          setTimeOut(() => {
            onFulfilled(this.value)
          })
        })
        this.onRejectedCallbacks.push(() => {
          setTimeOut(() => {
            onRejected(this.reason)
          })
        })
      }
    })
    return promise
  }
}
```
