## Create-React-App
### 修改脚手架配置



### Incompatible with Create-React-App 5.0: Invalid Options for PostCSS Loader

https://github.com/arackaf/customize-cra/issues/315

```JavaScript
// 将adjustStyleLoaders放到addLessLoader之后
adjustStyleLoaders(({ use: [, , postcss] }) => {
  const postcssOptions = postcss.options;
  postcss.options = { postcssOptions };
})
```