https://reactnavigation.org/docs/getting-started
https://reactnavigation.org/docs/stack-navigator
https://reactnavigation.org/docs/drawer-navigator


[Navigating without the navigation prop](https://reactnavigation.org/docs/navigating-without-navigation-prop/)

#### 3.1.1. 安装&配置

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

#### 3.1.2. Stack Navigator
 
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

#### 3.1.3. Drawer Navigator

https://reactnavigation.org/docs/drawer-navigator

```
npm install @react-navigation/drawer
```

1. First, install [`react-native-gesture-handler`](https://docs.swmansion.com/react-native-gesture-handler/) and [`react-native-reanimated`](https://docs.swmansion.com/react-native-reanimated/).
```Shell
npm install react-native-gesture-handler react-native-reanimated
```


> [!NOTE] TroubleShooting
> The Drawer Navigator supports both Reanimated 1 and Reanimated 2. If you want to use Reanimated 2, make sure to configure it following the [installation guide](https://docs.swmansion.com/react-native-reanimated/docs/fundamentals/installation).

2. To finalize installation of `react-native-gesture-handler`, add the following at the **top** (make sure it's at the top and there's nothing else before it) of your entry file, such as `index.js` or `App.js`:
```Shell
import 'react-native-gesture-handler';
```

> [!NOTE] Note
> If you are building for Android or iOS, do not skip this step, or your app may crash in production even if it works fine in development. This is not applicable to other platforms.

3.  If you're on a Mac and developing for iOS, you also need to install the pods (via [Cocoapods](https://cocoapods.org/)) to complete the linking.
```Shell
npx pod-install ios
```

