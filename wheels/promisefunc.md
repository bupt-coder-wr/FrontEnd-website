辅助函数 isPromise

```javascript
function isPromise(fn) {
  return (
    !!obj &&
    (typeof obj === 'object' || typeof obj === 'function') &&
    typeof obj.then === 'function'
  )
}
```

### promise.all(字节一面)

> Promise.all 特点：
>
> - 接受一个数组，每个 item 是一个 promise
> - 如果元素不是 Promise 对象，则使用 Promise.resolve 转成 Promise 对象
> - 返回一个新的 Promise
> - 等所有 Promise resolve 后 resolve 一个数组
> - 如果有一个 Promise reject，则 Promise 直接终止，返回一个 reject 的 Promise

```javascript
Promise.myAll = function (promises) {
  // return Promise
  return new Promise((resolve, reject) => {
    //校验入参
    if (!Array.isArray(promises)) {
      return reject(new TypeError('arguments must be an array'))
    }
    let resolvedPromiseCount = 0
    let promiseLength = promises.length
    let results = Array(promiseLength)
    promises.forEach((item, index) => {
      let curPromise = item
      // 非 Promise 使用 Promise.resolve 包装
      if (!isPromise(curPromise)) {
        curPromise = Promise.resolve(item)
      }
      curPromise.then(
        value => {
          resolvedPromiseCount++
          results[index] = value
          // 待所有 Promise 都 resolve 后 进行返回
          if (resolvedPromiseCount === promiseLength) {
            return resolve(results)
          }
        },
        error => {
          reject(error)
        }
      )
    })
  })
}
```

代码测试：

```javascript
let p1 = Promise.resolve('1 success')
let p2 = new Promise(resolve => {
  setTimeout(() => {
    resolve('p2 success')
  }, 2000)
})
let p3 = 'success'
Promise.myAll([p1, p2, p3]).then(
  res => {
    console.log(res)
  },
  error => {
    console.log(error)
  }
)
// [ '1 success', 'p2 success', 'success' ]
```

### promise.allSettled

> Promise.allSettled 特点
>
> - 接受一个数组，每个 item 是一个 Promise
> - 如果元素不是 Promise 对象，则使用 Promise.resolve 转成 Promise 对象
> - 返回一个新的 Promise
> - 等所有 Promise resolve 后 resolve 一个数组
> - 对于每个结果对象，都有一个 status 字符串。如果它的值为 fulfilled，则结果对象上存在一个 value 。如果值为 rejected，则存在一个 reason

```javascript
Promise.myAllSettled = function (promises) {
  // 返回一个新的 Promise
  return new Promise((resolve, reject) => {
    // 入参校验
    if (!Array.isArray(promises)) {
      return reject(new TypeError('Arguments must be an array'))
    }
    let settledPromiseCount = 0
    let promiseLength = promises.length
    let results = Array(promiseLength)

    function handleValue(status, value, index, results) {
      settledPromiseCount++
      let obj = { status }
      if (status === 'fulfilled') obj.value = value
      if (status === 'rejected') obj.reason = value
      results[index] = obj
    }
    promises.forEach((item, index) => {
      let curPromise = item
      // 处理非 Promise 元素
      if (!isPromise(curPromise)) {
        curPromise = Promise.resolve(item)
      }
      curPromise.then(
        value => {
          handleValue('fulfilled', value, index, results)
          if (settledPromiseCount === promiseLength) {
            return resolve(results)
          }
        },
        error => {
          handleValue('rejected', value, index, results)
          if (settledPromiseCount === promiseLength) {
            return resolve(results)
          }
        }
      )
    })
  })
}
```

测试用例：

```javascript
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('p1 reject')
  }, 2000)
})
let p2 = Promise.resolve('p2 success')
let p3 = 'p3'
Promise.myAllSettled([p1, p2, p3]).then(res => {
  console.log(res)
})
/**
 * [
 *   { status: 'rejected', reason: 'p1 reject' },
 *   { status: 'fulfilled', value: 'p2 success' },
 *   { status: 'fulfilled', value: 'p3' }
 * ]
 */
```

### promise.race

> Promise.allSettled 特点
>
> - 接受一个数组，每个 item 是一个 Promise
> - 如果元素不是 Promise 对象，则使用 Promise.resolve 转成 Promise 对象
> - 返回一个新的 Promise
> - 一旦迭代器中的某个 Promise 解决或拒绝，返回的 Promise 就会解决或拒绝

```javascript
Promise.myRace = function (promises) {
  // 入参校验
  if (!Array.isArray(promises)) {
    return reject('Argument must be an array')
  }
  promises.forEach(item => {
    let curPromise = item
    if (!isPromise(curPromise)) {
      curPromise = Promise.resolve(item)
    }
    curPromise.then(
      value => {
        return resolve(value)
      },
      error => {
        return reject(error)
      }
    )
  })
}
```

测试用例：

```javascript
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('error')
  }, 2000)
})
let p2 = Promise.resolve('p2 success')
Promise.myRace([p1, p2]).then(
  val => {
    console.log(val)
  },
  error => {
    console.log(error)
  }
)
// p2 success
```

### promise.finally

> 对于所有 fulfilled 和 rejected 和 Promise 都会执行 finally 中的函数

```javascript
Promise.prototype.myFinally = function (fn) {
  return this.then(
    res => Promise.resolve(fn()).then(() => res),
    err => Promise.resolve(fn()).then(() => throw err)
  )
}
```

### promise.cancel (字节一面)

> p1 = PromiseWithCancel(p)
> p1.cancel() => "p has been cancel"
> 封装一个 PromiseWithCancel 函数，该函数接收一个 Promise 参数，返回一个新的 Promise，该 Promise 具有 cancel 方法，可以暂停之前的 Promise

```javascript
let p = new Promise((resolve, reject) => {
  setTimeOut(() => {
    resolve('success')
  }, 2000)
})

function PromiseWithCancel(p) {
  p.cancel = function () {
    Promise.race[(p, Promise.reject())].catch(() => {
      console.log('p has been cancel')
    })
  }
  return p
}
```
