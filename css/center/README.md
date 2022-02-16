### 行内元素居中

页面结构

```html
<p class="box">Loading</p>
```

**水平居中**

```css
// 方式 1
.box {
  text-align: center;
}
// 方式 2
.box {
  display: flex;
  justify-content: center;
}
```

**垂直居中**

```css
// 方式 1
.box {
  height: 20px;
  line-height: 20px;
}
// 方式 2
.box {
  disaply: table-cell;
  vertical-align: middle;
}
```

### 块级元素居中

页面结构

```html
<div class="parent">
  <div class="child"></div>
</div>
```

**1. 自元素定长宽 + margin: auto**

```css
.parent {
  width: 100px;
  height: 100px;
}
.child {
  // key code
  margin: auto;
  width: 50px;
  height: 50px;
}
```

**2. 绝对定位 + top/bottom/left/right + margin**

```css
.parent {
  width: 100px;
  height: 100px;
}
.child {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0px;
  margin: auto;
}
```

**3. display: flex**

```css
// 子项可定长宽，可不定长宽
.parent {
  width: 100px;
  height: 100px;

  display: flex;
  justify-content: center;
  align-items: center;
}
.child {
  margin: auto;
  width: 50px;
  height: 50px;
}
```

**4. top/left + transform**

```css
// 子项可定长宽，可不定长宽
.parent {
  width: 100px;
  height: 100px;
}
.child {
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

**5. display:table-cell + vertical-align + inline-block**

```css
// 子项可定长宽，可不定长宽
.box {
  width: 100px;
  height: 100px;
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
.children-box {
  width: 50px;
  height: 50px;
  display: inline-block; // 可以换成margin: auto;
}
```
