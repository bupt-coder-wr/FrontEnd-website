### runPromiseAllWithLimit (字节一面)

```javascript
function runPromiseAllWithLimit(promises, limit) {
  // your code
  return new Promise(resolve => {
    let result = []
    let resolvePromiseLength = 0
    let curPromiseLength = 0
    function subFn() {
      promises.forEach((item, index) => {
        if (curPromiseLength < limit) {
          curPromiseLength++
          item().then(res => {
            resolvePromiseLength++
            curPromiseLength--
            subFn()
            result[index] = res
            if (resolvePromiseLength === promises.length) {
              resolve(result)
            }
          })
        }
      })
    }
    subFn()
  })
}
```
