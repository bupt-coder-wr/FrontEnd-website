## `this` 指向问题

> 参考文章：https://juejin.cn/post/6844904083707396109

`this` 的几种绑定方式：

- 默认绑定：非严格模式下 `this` 指向全局对象，严格模式下 `this` 绑定到 `undefined`
- 隐式绑定：当函数引用有上下文对象时，指向该对象

  `this` 永远指向最后调用它的那个对象（不考虑箭头函数

- 显示绑定：通过 `call`，`apply`，`bind` 调用

  如果 `call`，`apply`，`bind` 接受到的第一个参数是空或者 `null`，`undefined` 时，则会忽略这个参数  
  `forEach`, `map`, `filter` 函数的第二个参数也是能显示绑定 `this` 的

```javascript
var obj = {
  a: 1,
  foo: function (b) {
    b = b || this.a
    return function (c) {
      console.log(this.a + b + c)
    }
  },
}
var a = 2
var obj2 = { a: 3 }

obj.foo(a).call(obj2, 1)
obj.foo.call(obj2)(1)
// 6
// 6
```

- `new` 绑定

  通过 `new` 关键字形成的实例可以直接转换为对象

- 箭头函数绑定

  箭头函数内的`this` 由外层作用域决定，且指向函数定义时的 `this` 而非执行时。  
  作用域只有函数作用域和全局作用域
  箭头函数的 this 无法通过 `bind`，`call`，`apply` 来直接修改

> 此题对 _函数定义时，而非执行时_ 做出诠释

```javascript
var name = 'window'
function Person(name) {
  this.name = name
  this.foo1 = function () {
    console.log(this.name)
  }
  this.foo2 = () => {
    console.log(this.name)
  }
}
var person2 = {
  name: 'person2',
  foo2: () => {
    console.log(this.name)
  },
}
var person1 = new Person('person1')
person1.foo1()
person1.foo2()
person2.foo2()
```

## Event Loop

`Event Loop` 执行顺序

- 一开始整个脚本作为宏任务执行
- 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列
- 直到当前宏任务执行完，检查微任务列表，有则依次执行，直到全部执行完
- 执行浏览器 UI 线程的渲染工作
- 检查是否有 `Web Worker` 任务，有则执行
- 执行完本轮的宏任务，回到 2，依次循环，知道宏任务和微任务列表都为空

宏任务: `script`, `setTimeOut`, `setInterval`, `setImmediate`, `I/O`, `UI rendering`

微任务：`Promise.then()或 catch()`, `process.nextTick`, `fetch API`, `V8`的垃圾回收

任何一个非 `Promise` 对象都会包裹成 `Promise` 对象

```javascript
Promise.resolve()
  .then(() => {
    return new Error('error!!!')
  })
  .then(res => {
    console.log('then: ', res)
  })
  .catch(err => {
    console.log('catch: ', err)
  })
// "then: " "Error:error!!"
```

`async/await` 与 `Promise` 的转换

// async/await 代码

```javascript
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2')
}
async1()
console.log('start')
```

// Promise 代码

```javascript
async function async1() {
  console.log('async1 start')
  // 原来代码
  // await async2();
  // console.log("async1 end");

  // 转换后代码
  new Promise(resolve => {
    console.log('async2')
    resolve()
  }).then(res => console.log('async1 end'))
}
async function async2() {
  console.log('async2')
}
async1()
console.log('start')
```

`await` 后代码相当于 `Promise.then()`中的回调代码
