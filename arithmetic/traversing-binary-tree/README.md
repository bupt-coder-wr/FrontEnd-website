### 前序遍历

递归法

```javascript
const preorderTraversal = (root, res = []) => {
  if (!root) return
  res.push(root.val)
  preorderTraversal(root.left, res)
  preorderTraversal(root.right, res)
  return res
}
```

非递归法

```javascript
const preorderTraversal = (root, res = []) => {
  if (!root) return res
  let stack = []
  stack.push(root)
  while (stack.length) {
    const node = stack.pop()
    res.push(node.val)
    // 遍历是先左子节点，后右子节点，根据栈先进后出的特点故应该先进右子节点
    if (node.right) stack.push(node.right)
    if (node.left) stack.push(node.left)
  }
  return res
}
```

### 中序遍历

递归法

```javascript
const inorderTraversal = (root, res = []) => {
  if (!root) return
  inorderTraversal(root.left, res)
  res.push(root.val)
  inorderTraversal(root.right, res)
  return res
}
```

非递归法

```javascript
const inorderTraversal = (root, res = []) => {
  if (!root) return res
  let stack = []
  let cur = root
  // 中序遍历顺序为：左-中-右
  while (cur || stack.length) {
    // 先向左子树遍历, 到头后遍历右子树
    if (cur) {
      stack.push(cur)
      cur = cur.left
    } else {
      cur = stack.pop()
      res.push(cur)
      cur = cur.right
    }
  }
  return res
}
```

### 后序遍历

递归法

```javascript
const postorderTraversal = (root, res = []) => {
  if (!root) return
  postorderTraversal(root.left, res)
  postorderTraversal(root.right, res)
  res.push(val)
  return res
}
```

非递归法

```javascript
const postorderTraversal = (root, res = []) => {
  if (!root) return res
  let stack = []
  stack.push(root)
  while (stack.length) {
    let node = stack.pop()
    res.unshift(node.val)
    // 后序遍历顺序是：左-右-中
    if (node.left) stack.push(node.left)
    if (node.right) stack.push(node.right)
  }
  return res
}
```

### 层序遍历

```javascript
const levelOrder = (root, res = []) => {
  if (!root) return res
  let list = []
  list.push(root)
  while (list.length) {
    const len = list.length
    let tmp = []
    for (let i = 0; i < len; i++) {
      const node = list.shift()
      tmp.push(node.val)
      if (node.left) list.push(node.left)
      if (node.right) list.push(node.right)
    }
    res.push(tmp.slice())
  }
  return res
}
```
