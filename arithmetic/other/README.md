### 洗牌算法

```javascript
function shuffle(arr) {
  const len = arr.length
  for (let i = 0; i < len; i++) {
    const p = parseInt(Math.random() * len
    let tmp = arr[i]
    arr[i] = arr[p]
    arr[p] = tmp
  }
}
```

### 下划线命名转驼峰命名

```javascript
function toHump(name) {
  return name.replace(/\_(\w)/g, function (all, letter) {
    return letter.toUpperCase()
  })
}
```
