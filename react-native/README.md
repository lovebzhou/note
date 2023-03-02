
## 1. 概述

- [React Native官网](https://reactnative.dev/)
- [React Native中文网](https://reactnative.cn/)
- [React Native 学习资源精选仓库(汇聚知识，分享精华)](https://github.com/crazycodeboy/react-native-awesome)
- [Awesome React Native](https://github.com/jondot/awesome-react-native#open-source-apps)


## 2. 开发流程

### 2.1. 环境搭建

- [搭建开发环境](https://reactnative.cn/docs/environment-setup)

### 2.2. 在设备上运行

**iOS**

1. 苹果开发者账号加入Huoban Team
2. 添加iOS测试设备ID
3. 打开XCode项目，修改Signing & Capabilities / Bundle Identifier：`com.huoban.demo.helloRN`

> [在设备上运行](https://reactnative.cn/docs/running-on-device)

```Shell
# 查看iOS Devices
xcrun simctl list devices
# 制定设备类型
npx react-native run-ios --simulator "iPhone 4s"

# 查看Android Devices
adb devices
```
**Android**

1. 打开`开发者选项`[小米](https://jingyan.baidu.com/article/a681b0de7c39327a184346c6.html)
2. 在开`发者选项中`，打开`USB调试`、`USB安装`

### 2.3. Start


```
# https://reactnative.cn/docs/environment-setup
# 1. 创建新项目
npx react-native init Hello

# 2. 编译并运行 React Native 应用
cd Hello
npm run ios

```

**问题**

1. error rbenv: version `2.7.6' is not installed 

```
// 安装rbenv: https://github.com/rbenv/rbenv
brew install rbenv ruby-build

// run this and follow the printed instructions:
rbenv init


// list latest stable versions:
rbenv install -l
// list all local versions:
rbenv install -L
// install a Ruby version:
rbenv install 3.1.2

// set the default Ruby version for this machine
rbenv global 3.1.2
// set the Ruby version for this directory
rbenv local 3.1.2   

// 查看全部ruby版本
rbenv versions

// 查看当前使用ruby版本
rbenv version

```

### 2.4. 调试

#### 2.4.1. Chrome开发者工具

在开发者菜单中选择"Debug JS Remotely"选项，即可以开始在 Chrome 中调试 JavaScript 代码。点击这个选项的同时会自动打开调试页面 [http://localhost:8081/debugger-ui](http://localhost:8081/debugger-ui).(如果地址栏打开的是 ip 地址，则请自行改为 localhost)
在 Chrome 的菜单中选择`Tools → Developer Tools`可以打开开发者工具，也可以通过键盘快捷键来打开（Mac 上是**`Command`**`⌘` + **`Option`**`⌥` + **`I`**，Windows 上是**`Ctrl`** + **`Shift`** + **`I`**或是 F12）。打开[有异常时暂停（Pause On Caught Exceptions）](http://stackoverflow.com/questions/2233339/javascript-is-there-a-way-to-get-chrome-to-break-on-all-errors/17324511#17324511)选项，能够获得更好的开发体验。


> [!NOTE] 注意：
> 注意：使用 Chrome 调试目前无法观测到 React Native 中的网络请求，你可以使用功能更强大的第三方的[react-native-debugger](https://github.com/jhen0409/react-native-debugger)或是官方的[flipper](https://fbflipper.com/)（注意仅能在 0.62 以上版本可用）来观测。
> 启动：open "rndebugger://set-debugger-loc?host=localhost&port=8081"

#### 2.4.2. React Developer Tools
你可以使用[独立版 React 开发者工具(不是 chrome 的插件)](https://github.com/facebook/react/tree/main/packages/react-devtools)来调试 React 组件层次结构。要使用它，请全局安装`react-devtools`包:

> 注意：react-devtools v4 需要 react-native 0.62 或更高版本才能正常工作。

```Shell
npm install -g react-devtools
```

> 译注：react-devtools 依赖于 electron，而 electron 需要到国外服务器下载二进制包，所以国内用户这一步很可能会卡住。此时请在`环境变量`中添加 electron 专用的国内镜像源：`ELECTRON_MIRROR="https://npm.taobao.org/mirrors/electron/"`，然后再尝试安装 react-devtools。

安装完成后在命令行中执行`react-devtools`即可启动此工具：

```Shell
react-devtools
```

>chrome://inspect/#devices


**iOS**

1. iOS Open React DevTools

> error Browser exited with error:, Error: invalid url, missing http/https protocol

[使用新的 Hermes 引擎](https://reactnative.cn/docs/hermes)

**Android**

1. 设备无响应

```
通过Android Device Manager冷启动

// 命令行列出设备列表 
adb devices -l
```

### 2.5. Metro

https://github.com/facebook/metro

### 2.5. TroubleShooting

#### 2.5.1. Another debugger is already connected
多处启动了`React Native包生成工具`
- 命令行：npm run ios
	- 直接关闭终端
- VS Code
	- 点击状态栏 启动/停止
### 2.5.2. Runtime is not ready for debugging. Make sure Metro is runing
Runtime is not ready for debugging. Make sure Metro is running.


## 3. 基础设施

### 3.1. Navigation


#### 3.1.1. React Navigation
https://reactnavigation.org/docs/getting-started
https://reactnavigation.org/docs/stack-navigator
https://reactnavigation.org/docs/drawer-navigator

1. **Installation**
```Shell
# Install the required packages in your React Native project
npm install @react-navigation/native

# Installing dependencies into a bare React Native project
npm install react-native-screens react-native-safe-area-context
```

2. **Wrapping your app in `NavigationContainer`**
```JavaScript
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>{/* Rest of your app code */}</NavigationContainer>
  );
}
```
3. **Installing the native stack navigator library**
```Shell
npm install @react-navigation/native-stack
```

4. **Update CRA init files**
- iOS
```Shell
npx pod-install ios
```
- Android
`react-native-screens` package requires one additional configuration step to properly work on Android devices. Edit `MainActivity.java` file which is located in `android/app/src/main/java/<your package name>/MainActivity.java`.
Add the following code to the body of `MainActivity` class:
```Java
@Overrideprotected void onCreate(Bundle savedInstanceState) {  super.onCreate(null);}
```
and make sure to add the following import statement at the top of this file below your package statement:
```Java
import android.os.Bundle;
```

5. **Creating a native stack navigator**
```JavaScript
// In App.js in a new project

import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  );
}

const Stack = createNativeStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```
####  3.1.2. React Native Navigation
https://wix.github.io/react-native-navigation/docs/installing/

- NPM
```Shell
npm install --save react-native-navigation
```
- Installing with `npx rnn-link`
```Shell
npx rnn-link
```
- CocoaPods
```Shell
# for iOS
pod install --project-directory=ios
```
- Update index.js
```diff
+import { Navigation } from "react-native-navigation";
-import {AppRegistry} from 'react-native';
import App from "./App";
-import {name as appName} from './app.json';

-AppRegistry.registerComponent(appName, () => App);
+Navigation.registerComponent('com.myApp.WelcomeScreen', () => App);

+Navigation.events().registerAppLaunchedListener(() => {
+   Navigation.setRoot({
+     root: {
+       stack: {
+         children: [
+           {
+             component: {
+               name: 'com.myApp.WelcomeScreen'
+             }
+           }
+         ]
+       }
+     }
+  });
+});
```

### 3.2. Net

### 3.3. Storage

### 3.4. Theme

### 3.5. Notification Push

### 3.6. Share

### 3.7. 监控

### 3.8. Location

## 4. Component

### 4.1. View
https://reactnative.cn/docs/view

## 5. 设计

### 5.1.  样式

### 5.2. 布局
https://reactnative.cn/docs/flexbox


## 6. 性能优化

### 6.1. Launch Screen


```Objective-C
// https://reactnative.cn/docs/publishing-to-app-store
// Place this code after "[self.window makeKeyAndVisible]" and before "return YES;"  
UIStoryboard *sb = [UIStoryboard storyboardWithName:@"LaunchScreen" bundle:nil];  
UIViewController *vc = [sb instantiateInitialViewController];  
rootView.loadingView = vc.view;
```




