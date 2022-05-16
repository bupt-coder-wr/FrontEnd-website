### 洗牌算法

```javascript
function shuffle(arr) {
  const len = arr.length
  for (let i = 0; i < len; i++) {
    const p = parseInt(Math.random()) * len
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

### 类数组转数组

1. Array.prototype.slice.call()

```javascript
target = Array.prototype.slice.call(source)
```

2. Array.from()

```javascript
target = Array.from(source)
```

3. 扩展运算符

```javascript
target = [...source]
```

### 对象判空

```javascript
// way 1
function isEmpty(obj) {
  for (let item in obj) return true
  return false
}

// way 2
JSON.stringify(obj) === '{}'

// way 3
Object.keys(obj).length === 0
```
