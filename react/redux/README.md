### 核心概念

- store

- action

- reducer

### 数据流

`UI` -> `action` -> `reducer` -> `store` -> `UI`

### 三大原则

- 单一数据源
- 状态只读
- 纯函数

### 核心 API

- createStore()
  > 创建数据仓库

```javascript
const store: Object = createStore(reducer)
```

- getState()
  > 获取 store 中当前的状态。
- dispatch(action)
  > 派发 action，唯一改变 store 中数据的方式。
- subscribe(listener)
  > 注册一个监听者，它在 store 发生变化时被调用。
- replaceReducer()
  > 更新当前 store 里的 reducer，一般只会在开发模式中调用该方法。
