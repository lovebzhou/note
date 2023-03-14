#### 修改less文件内资源引用根路径

1. package.json
```JSON
{
	"scripts": {
		"start:pre": "REACT_APP_API=preview react-app-rewired --max-old-space-size=4096 start",
		"start:prod": "REACT_APP_API=preview react-app-rewired --max-old-space-size=4096 start",
	}
}
```


2. config-overrides.js增加
```JavaScript


module.exports = override(
	addLessLoader({
		lessOptions: {
			javascriptEnabled: true,
			modifyVars: {
				'asset-path': process.env.REACT_APP_API === 'preview' ? '/app1002453/static/' : '/static',
				// 'asset-path': '/static',	
			},
		},
	}),
)
```
3. 修改less文件
```Less
@iconfont-path: '@{asset-path}/font';
@iconfont-version: 'mqt7az187';
@font-face {
	font-family: 'hb-system';
	src: url('@{iconfont-path}/hb-system.eot?@{iconfont-version}');

}
```
