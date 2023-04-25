# SVG学习笔记

- [SVG 从入门到后悔，怎么不早点学起来（图解版）](https://juejin.cn/post/7118985770408345630)
- [SVG教程](https://www.w3school.com.cn/svg/index.asp)

## 1. 使用方式

### 浏览器直接打开
```xml
<?xml version="1.0" standalone="no"?>
<svg width="300" height="300" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <rect width="200" height="100"></rect>
</svg>
```
必须使用 `xmlns="http://www.w3.org/2000/svg"`。

### 内嵌到HTML中
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>svg demo</title>
</head>
<body>
  <div>
    <!-- 内嵌到 HTML 中 -->
    <svg width="100%" height="100%" version="1.1">
      <circle cx="50" cy="50" r="50" fill="hotpink"></circle>
    </svg>
  </div>
</body>
</html>
```

### CSS背景图
```css
<style>
.svg_bg_img {
  width: 100px;
  height: 100px;
  background: url('./case1.svg') no-repeat;
  background-size: 100px 100px;
}
</style>

<div class="svg_bg_img"></div>
```

### 使用 img 标签引入
```html
<img src="./case1.svg" width="100" height="100">
```

### 使用 iframe 标签引入
```html
<iframe
  src="./case1.svg"
  width="100"
  height="100"
></iframe>
```

### 使用 embed 标签引入
```html
<embed
  src="./case1.svg"
  width="100"
  height="100"
/>
```

### 使用 object 标签引入
```html
<object
  data="./case1.svg"
  type="image/svg+xml"
  codebase="http://www.adobe.com/svg/viewer/install"
></object>
```
## 2. 基础图形
### 矩形：rect
-   `x`: 左上角x轴坐标
-   `y`: 左上角y轴坐标
-   `width`: 宽度
-   `height`: 高度
-   `rx`: 圆角，x轴的半径
-   `ry`: 圆角，y轴的半径
```xml
<svg style="border: 1px solid red;">
	<rect
	x="30"
	y="15"
	rx="30"
	width="200"
	height="100"
	fill="orange"
	stroke="blue"
	stroke-width="5"
	/>
</svg>
```

### 圆形：circle
-   `cx`: 圆心在x轴的坐标
-   `cy`: 圆心在y轴的坐标
-   `r`: 半径
```xml
<svg width="300" height="300" style="border: 1px solid red;">
	<circle
	cx="100"
	cy="100"
	r="60"
	fill="red"
	stroke="blue"
	stroke-width="10"
	/>
</svg>
```
### 椭圆：ellipse
-   `cx`: 圆心在x轴的坐标
-   `cy`: 圆心在y轴的坐标
-   `rx`: x轴的半径
-   `ry`: y轴的半径
```xml
<svg width="300" height="300" style="border: 1px solid red;">
	<ellipse
	cx="120"
	cy="100"
	rx="100"
	ry="60"
	fill="yellow"
	stroke="red"
	stroke-width="10"
	/>
</svg>
```
### 直线：line
-   `x1`: 起始点x坐标
-   `y1`: 起始点y坐标
-   `x2`: 结束点x坐标
-   `y2`: 结束点y坐标
-   `stroke`: 描边颜色
```xml
<svg width="300" height="300" style="border: 1px solid red;">
	<line
	x1="30"
	y1="40"
	x2="200"
	y2="180"
	fill="yellow"
	stroke="blue"
	stroke-width="10"
	/>
</svg>
```
### 折线：polyline
-   `points`: 点集
-   `stroke`: 描边颜色
- fill: 填充颜色
	- `none`：用于绘制折线，默认填充黑色

```xml
<svg width="300" height="300" style="border: 1px solid red;">
	<polyline
	points="10 10, 200 80, 230 230"
	fill="yellow"
	stroke="blue"
	stroke-width="5"
	></polyline>
</svg>
```

### 多边形 polygon
-   `points`: 点集
-   `stroke`: 描边颜色
-   `fill`: 填充颜色
```xml
<svg width="300" height="300" style="border: 1px solid red;">
	<polygon
	points="10 10, 200 80, 230 230"
	fill="yellow"
	stroke="blue"
	stroke-width="5"
	/>
</svg>
```


