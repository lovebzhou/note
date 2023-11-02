CSS Flex 布局（也称为弹性盒子布局）是一种用于在网页中设计和排列元素的现代 CSS 布局模型。它提供了更直观和灵活的方式来创建复杂的布局，特别适用于构建响应式设计和灵活的用户界面。Flex 布局基于容器和其内部的弹性子元素。

以下是关于 CSS Flex 布局的详细介绍：

**基本概念：**

- **Flex 容器（Flex Container）：** 包含了一个或多个子元素的容器，它的 `display` 属性被设置为 `flex` 或 `inline-flex`。
  
  ```css
  .flex-container {
    display: flex; /* 或 inline-flex */
  }
  ```

- **Flex 子元素（Flex Items）：** 容器内部的子元素，它们会根据容器的规则进行排列。

**Flex 容器的属性：**

1. **`flex-direction`：** 指定了子元素的排列方向，可以是 `row`（水平排列，默认）、`column`（垂直排列）、`row-reverse`（水平反向排列）或 `column-reverse`（垂直反向排列）。

2. **`flex-wrap`：** 定义了子元素是否换行，可以是 `nowrap`（不换行，默认）、`wrap`（换行）或 `wrap-reverse`（反向换行）。

3. **`flex-flow`：** 是 `flex-direction` 和 `flex-wrap` 的缩写属性，通常一起使用。

4. **`justify-content`：** 控制了子元素在主轴上的对齐方式，可以是 `flex-start`（起点对齐，默认）、`flex-end`（终点对齐）、`center`（居中对齐）、`space-between`（两端对齐，子元素之间平均分布）或 `space-around`（子元素周围平均分布）。

5. **`align-items`：** 控制了子元素在交叉轴上的对齐方式，可以是 `flex-start`、`flex-end`、`center`、`baseline`（基线对齐，子元素的基线对齐）或 `stretch`（拉伸填满容器，默认）。

6. **`align-content`：** 用于多行排列的子元素在交叉轴上的对齐方式，可以是 `flex-start`、`flex-end`、`center`、`space-between`、`space-around` 或 `stretch`。

**Flex 子元素的属性：**

1. **`flex`：** 设置子元素的扩展比例，控制它们在可用空间内的分配。例如，`flex: 1` 表示所有子元素平均分配可用空间。

2. **`order`：** 定义了子元素的显示顺序，数值小的在前面，默认为 0。

3. **`align-self`：** 用于覆盖父容器上的 `align-items` 属性，单独控制某个子元素在交叉轴上的对齐方式。

**示例：**

```css
.container {
  display: flex;
  flex-direction: row; /* 水平排列 */
  justify-content: space-between; /* 两端对齐 */
  align-items: center; /* 垂直居中对齐 */
}

.item {
  flex: 1; /* 平均分配可用空间 */
  order: 2; /* 显示顺序 */
  align-self: flex-start; /* 自定义子元素的交叉轴对齐方式 */
}
```

**主要特点：**

- 自适应：Flex 布局适应不同尺寸的屏幕和容器，使得布局更具响应性。
- 简单易用：相对于传统的 CSS 布局方法，Flex 布局的语法更加简洁和直观。
- 支持多行布局：可以轻松实现多行的布局效果，而不需要复杂的浮动或定位。

CSS Flex 布局是现代网页设计中的重要工具，可以帮助开发人员更轻松地构建复杂的布局，适应不同的屏幕和设备，并提供更好的用户体验。它在响应式设计、导航菜单、卡片布局等方面都得到广泛应用。