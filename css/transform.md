CSS `transform` 是一种用于在网页中对元素进行二维和三维变换的 CSS 属性。通过 `transform`，您可以修改元素的平移、旋转、缩放和倾斜等属性，从而实现各种动画效果和页面布局的调整，而无需改变文档流。

以下是关于 CSS `transform` 的详细介绍：

**基本语法：**

```css
element {
  transform: transform-function;
}
```

- `element`：要应用变换的 HTML 元素。
- `transform`：CSS 属性，用于指定一个或多个变换函数。
- `transform-function`：一个或多个变换函数，可以组合使用，每个函数之间用空格分隔。常见的变换函数包括：
  - `translate(x, y)`：平移元素，其中 `x` 和 `y` 分别是水平和垂直方向上的偏移量。
  - `rotate(angle)`：旋转元素，其中 `angle` 是旋转角度（正值表示顺时针，负值表示逆时针）。
  - `scale(x, y)`：缩放元素，其中 `x` 和 `y` 分别是水平和垂直方向上的缩放比例。
  - `skew(x-angle, y-angle)`：倾斜元素，其中 `x-angle` 和 `y-angle` 分别是水平和垂直方向上的倾斜角度。

**示例：**

```css
/* 平移元素 */
div {
  transform: translate(50px, 20px);
}

/* 旋转元素 */
div {
  transform: rotate(45deg);
}

/* 缩放元素 */
div {
  transform: scale(1.5, 2);
}

/* 倾斜元素 */
div {
  transform: skew(30deg, 20deg);
}
```

**注意事项：**

- `transform` 属性可以组合多个变换函数，它们将按照声明的顺序依次应用于元素。
- 变换函数的参数可以是长度单位（如像素 `px`、百分比 `%`）、角度单位（如度 `deg`）、无单位数字等。
- 您可以使用负值来实现相反的变换效果。
- 三维变换也是可能的，例如 `translate3d(x, y, z)`、`rotate3d(x, y, z, angle)` 和 `scale3d(x, y, z)`。
- `transform-origin` 属性用于指定变换的原点，默认为元素的中心点。
- CSS `transform` 不会影响文档流，所以其他元素不会受到变换的影响。
- 变换通常与 CSS 动画和过渡一起使用，以创建平滑的动画效果。

总的来说，CSS `transform` 是一个强大的工具，可以用于改变元素的外观和位置，实现各种动画效果和交互效果，而不影响文档布局。它在现代网页开发中广泛应用，用于创建各种用户界面和动态效果。