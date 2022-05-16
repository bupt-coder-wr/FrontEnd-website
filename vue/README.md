### Vue 传参方式

> https://juejin.cn/post/6844903887162310669#comment

- props / $emit

父组件向子组件传值

```html
// parent.vue
<template>
  <div>
    <article :articleList="articleList"></article>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        articleList: ['红楼梦', '三国演义', '西游记'],
      }
    },
    component: { article },
  }
</script>
```

```html
// article.vue
<template>
  <div>
    <span v-for="item in articleList" :key="item">{{item}}</span>
  </div>
</template>
<script>
  export default {
    props: ['articleList'],
  }
</script>
```

子组件向父组件传值

```html
// 父组件中
<template>
  <div class="section">
    <com-article
      :articles="articleList"
      @onEmitIndex="onEmitIndex"
    ></com-article>
    <p>{{currentIndex}}</p>
  </div>
</template>

<script>
  import comArticle from './test/article.vue'
  export default {
    name: 'HelloWorld',
    components: { comArticle },
    data() {
      return {
        currentIndex: -1,
        articleList: ['红楼梦', '西游记', '三国演义'],
      }
    },
    methods: {
      onEmitIndex(idx) {
        this.currentIndex = idx
      },
    },
  }
</script>
```

- `$parent` `$children`

$parent 获取到的为对象，根 Vue 实例的父元素为 undefined
$children 获取到的为数组

- provide / inject

非相应式

- ref / refs
- eventBus

$emit发送事件，$on 接受事件，$off 取消事件

- Vuex

state => data
getters => computed
mutations => methods 使用 commit 触发
actions => 异步 mutation 使用 dispatch 触发
modules => 将 store 模块化

- localStorage / sessionStorage

window.localStorage.setItem(key,value)
window.localStorage.getItem(key)

- $attrs / $listeners

### Vue-Router

1. hash

- 路径中带有#
- 路由参数不会带到服务端
- 兼容性好
- 主要通过`hashchange`事件进行监听

2. history

- 需要后端进行适配，未适配到的路由显示 404
- API 主要分为两类：
  - 修改路由：pushState()，replaceState()
  - 切换路由：go(),back(),forword()

### Virtual Dom 理解

- 无需手动操作 Dom，即可完成 diff
- 跨平台：本质上就是 Javascript 对象，真实 Dom 依赖浏览器平台。

### Diff

> https://juejin.cn/post/6994959998283907102#comment
