任何容器都可以指定为 flex 布局

```css
.box {
  display: flex;
}
```

行内元素也可以使用 `flex` 布局

```css
.box {
  display: inline-flex;
}
```

`Webkit` 内核的浏览器，必须加上 `-webkit` 前缀

```css
.box {
  display: -webkit-flex;
  display: flex;
}
```

容器属性

- `flex-direction`
  决定主轴的方向
  - `row`
  - `row-reverse`
  - `column`
  - `column-reverse`
- `flex-wrap`
- `flex-flex`
- `justify-content`
- `align-items`
- `align-content`

项目属性

- `order`
  定义项目的排列顺序
- `flex-grow`
  定义项目的放大比例，默认为 0，即如果存在剩余空间，也不放大。
- `flex-shrink`
  定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小。
- `flex-basis`
  定义了在分配多余空间之前，项目占据的主轴空间
- `flex`
  `flex-grow`, `flex-shrink` 和 `flex-basis` 的简写，默认值为 0 1 auto
- align-self
  允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性

> 参考文章: http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
