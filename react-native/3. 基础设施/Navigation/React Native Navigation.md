[文档](https://wix.github.io/react-native-navigation/docs/installing/)

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
