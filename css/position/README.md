### Position 取值

- static

  正常布局行为，及元素在文档常规流中当前的布局位置。此时 `top`, `right`, `bottom`, `left`, `z-index` 无效。

- relative

  元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置

- absolute

  脱离文档流，不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素。

  绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。（BFC）

- fixed

  脱离文档流，不为元素预留空间，通过指定元素相对于屏幕视口（viewport）的位置来指定元素位置。

- sticky

  根据正常文档流进行定位，然后相对它的最近滚动祖先和 containing block (最近块级祖先 nearest block-level ancestor)，包括 table-related 元素，基于 top, right, bottom, 和 left 的值进行偏移。
