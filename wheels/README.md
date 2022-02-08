### debounce

```javascript
const debounce = (fn, wait = 300) => {
  let timer = null
  return function (...args) {
    if (timer) clearTimeOut(timer)
    timer = setTimeOut(() => {
      fn.apply(this, args)
    }, wait)
  }
}
```

### throttle

```javascript
const throttle = (fn, wait = 300) => {
  let timer = null
  return function (...args) {
    if (timer) return
    timer = setTimeOut(() => {
      fn.apply(this, args)
      timer = null
    }, wait)
  }
}
```

### call

```javascript
Function.prototype.myCall = (context, ...args) => {
  if (typeof context !== 'function') {
    throw new TypeError('')
  }
  context = context || window
  context.fn = this
  let result = context.fn(...args)
  delete context.fn
  return result
}
```

### apply

```javascript
Function.prototype.myApply = (context, ...args) => {
  if (typeof context !== 'function') {
    throw new TypeError('')
  }
  context = context || window
  context.fn = this
  const args = [...arguments].slice(1)
  let result
  if (args) {
    result = context.fn(...args)
  } else {
    result = context.fn()
  }
  delete context.fn
  return result
}
```

### bind

```javascript
Function.prototype.myBind = (context, ...args) => {
  if (typeof context !== 'function') {
    throw new TypeError('')
  }
  const self = this
  return function F() {
    if (this instanceof F) {
      return new self(...args, ...arguments)
    } else {
      return self.apply(context, args.concat(arguments))
    }
  }
}
```

### new

```javascript
function create(Context, ...args) {
  let obj = {}
  obj.__proto__ = Context.prototype
  let result = Context.apply(this, args)
  // 若 Context 存在返回值，则返回 result
  // 若 Context 不存在返回值，则返回 obj
  return typeof result === 'object' ? result : obj
}
```

### setInterval

```javascript
const mySetInterval = (fn, wait = 300) => {
  let timerId = null
  const interval = () => {
    timerId = setTimeOut(interval, wait)
    fn()
  }
  timerId = setTimeOut(interval, wait)
  return () => clearTimeOut(timerId)
}
```

### instanceof

```javascript
const myInstanceof = (left, right) => {
  let proto = Object.getPrototypeOf(left)
  let prototype = right.prototype
  while (true) {
    if (!proto) return false
    if (proto === prototype) return true
    proto = Object.getPrototypeOf(proto)
  }
}
```

### deepClone

```javascript
const deepClone = target => {
  if (typeof target !== 'object') return target
  let result = target instanceof Array ? [] : {}
  for (let key in target) {
    result[key] = typeof key === 'object' ? deepClone(target[key]) : target[key]
  }
  return result
}
```
