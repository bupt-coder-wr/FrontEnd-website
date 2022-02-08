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

### promise.all

### promise.allSettled

### promise.race

### promise.finally

### promise.cancel
