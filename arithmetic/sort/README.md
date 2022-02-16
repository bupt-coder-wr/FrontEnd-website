## 快速排序

```javascript
function quickSort(arr, L, R) {
  if (L >= R) return
  let i = L
  let j = R
  let pivot = arr[i]
  while (i < j) {
    while (i < j && arr[j] >= pivot) {
      j--
    }
    if (i < j) {
      arr[i++] = arr[j]
    }
    while (i < j && arr[i] < pivot) {
      i++
    }
    if (i < j) {
      arr[j--] = arr[i]
    }
  }
  arr[i] = pivot
  quickSort(arr, L, i - 1)
  quickSort(arr, i + 1, R)
}
```

平均时间复杂度：`O(nlog n)`

最差情况时间复杂度：`O(n^2)`

最好情况时间复杂度：`O(nlog n)`

稳定性：`不稳定`

## 堆排序

```javascript
function swap(tree, i, j) {
  let tmp = tree[i]
  tree[i] = tree[j]
  tree[j] = tmp
}
/**
 * 递归法
 */
function heapify(tree, n, i) {
  // 递归出口，在最大值节点大于节点数时，递归结束
  if (i >= n) return
  // 分别获取左右孩子的索引
  let c1 = 2 * i + 1
  let c2 = 2 * i + 2
  let max = i
  // 找出当前堆的最大值
  if (c1 < n && tree[max] < tree[c1]) {
    max = c1
  }
  if (c2 < n && tree[max] < tree[c2]) {
    max = c2
  }
  // 将最大值放堆顶
  // 这是一个从上往下的过程，max和i数值发生交换，但是索引不变，此时 max 还指向 c1 或 c2 的位置,故仍以 max 为节点 heapify
  if (max !== i) {
    swap(tree, i, max)
    heapify(tree, n, max)
  }
}
/**
 * 迭代法
 */
function heapify(tree, n, i) {
  const max = tree[i]
  // 从 i 节点的左子节点开始
  for (let k = 2 * i + 1; k < n; k = 2 * k + 1) {
    // 如果左子节点小于右子节点，寻找最大子节点
    if (k + 1 < n && tree[k] < tree[k + 1]) {
      k++
    }
    if (tree[k] > tmp) {
      // 此时k为最大节点，继续向下堆化
      swap(tree, i, k)
      i = k
    } else {
      break
    }
  }
}
function buildHeap(tree, n) {
  let lastNode = n - 1
  let parent = Math.floor((lastNode - 1) / 2)
  for (let i = parent; i >= 0; i--) {
    heapify(tree, n, i)
  }
}
function heapSort(tree, n) {
  // 构建一个堆，此时满足堆的特性，即
  // tree为完全二叉树；parent > children
  buildHeap(tree, n)
  // 将根节点和最后一个节点做交换，即将最大值放在最后
  for (let i = n - 1; i >= 0; i--) {
    swap(tree, i, 0)
    heapify(tree, i, 0)
  }
}
```

平均时间复杂度：`O(nlog n)`

最差情况时间复杂度：`O(nlog n)`

最好情况时间复杂度：`O(nlog n)`

稳定性：`不稳定`

## 归并排序

```javascript
// 合并
function merge(arr, L, M, R) {
  let leftSize = M - L
  let rightSize = R - M + 1
  let left = arr.slice(L, L + leftSize)
  let right = arr.slice(M)
  let i = 0,
    j = 0,
    k = L
  while (i < leftSize && j < rightSize) {
    if (left[i] <= right[j]) {
      arr[k++] = left[i++]
    } else {
      arr[k++] = right[j++]
    }
  }
  while (i < leftSize) {
    arr[k++] = left[i++]
  }
  while (j < rightSize) {
    arr[k++] = right[j++]
  }
}
// 分治
function mergeSort(arr, L, R) {
  if (L === R) {
    return
  } else {
    let M = Math.floor((L + R) / 2)
    mergeSort(arr, L, M)
    mergeSort(arr, M + 1, R)
    merge(arr, L, M + 1, R)
  }
}
```

平均时间复杂度：`O(nlog n)`

最差情况时间复杂度：`O(nlog n)`

最好情况时间复杂度：`O(nlog n)`

稳定性：`稳定`

## 插入排序

```javascript
function insert(arr, n) {
  const key = arr[n]
  let i = n
  while (arr[i - 1] > key) {
    arr[i] = arr[i - 1]
    i--
    if (i == 0) {
      break
    }
  }
  arr[i] = key
}
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    insert(arr, i)
  }
}
```

平均时间复杂度：`O(n^2)`

最差情况时间复杂度：`O(n^2)`

最好情况时间复杂度：`O(n)`

稳定性：`稳定`

## 冒泡排序

```javascript
// 一次冒泡
function bubble(arr, n) {
  let tmp
  for (let i = 0; i < n - 1; i++) {
    if (arr[i] > arr[i + 1]) {
      tmp = arr[i]
      arr[i] = arr[i + 1]
      arr[i + 1] = tmp
    }
  }
}

// 冒泡排序
function bubbleSort(arr, n) {
  for (let i = n; i >= 1; i--) {
    bubble(arr, i)
  }
}
```

平均时间复杂度：`O(n^2)`

最差情况时间复杂度：`O(n^2)`

最好情况时间复杂度：`O(n)`

稳定性：`稳定`

## 选择排序

```javascript
function findMaxPos(arr, n) {
  let max = arr[0]
  let pos = 0
  for (let i = 0; i < n; i++) {
    if (arr[i] > max) {
      max = arr[i]
      pos = i
    }
  }
  return pos
}

function selectSort(arr, n) {
  // 长度为n，最大下标应该为n-1
  while (n - 1) {
    let pos = findMaxPos(arr, n)
    let tmp = arr[pos]
    arr[pos] = arr[n - 1]
    arr[n - 1] = tmp
    n--
  }
}
```

平均时间复杂度：`O(n^2)`

最差情况时间复杂度：`O(n^2)`

最好情况时间复杂度：`O(n)`

稳定性：`不稳定`

> 参考文档：
> https://www.runoob.com/w3cnote/quick-sort-2.html
