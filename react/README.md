### 为什么只在顶层使用 Hook

单个组件中可以使用多个 State Hook 或 Effect Hook，那么 React 怎么知道那个 state 对应哪个 `useState`？

答案是 React 靠的是 Hook 调用的顺序。只要 Hook 的调用顺序在多次渲染之间保持一致，React 就能正确地将内部 state 和对应的 Hook 进行关联。
