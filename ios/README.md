## 1. 概述


## 2. 开发环境

### Apple Developer
注册开发者账号，创建真机运行、打包、发布的证书。
https://developer.apple.com

### XCode
https://developer.apple.com/xcode/

### CocoaPods
依赖包管理工具。

https://cocoapods.org/

```Shell
# 安装
sudo gem install cocoapods

# 初始化:create a Podfile with smart defaults
pod init

# install the dependencies in your project
pod install

# 打开XCode工作区
open App.xcworkspace
```

## 构建 & 运行

#### Fastlane

```Shell
# 安装开发证书
fastlane match develop

```

### 分发

- [Find the best way to reach your users.](https://developer.apple.com/business/distribute/)
- [Distributing Custom Apps](https://developer.apple.com/custom-apps/)


#### XCode Cloud

https://developer.apple.com/cn/xcode-cloud/

Xcode Cloud 是专为 Apple 开发者设计的一项内置于 Xcode 中的持续集成和交付服务。


### 参考资料

- [iOS 图标&启动图生成器](https://github.com/hxsxyz/QiAppIconGenerator)
- [iOS应用发布方式盘点+苹果商务详解](https://www.jianshu.com/p/c8361a83a338)
- [iOS商务管理分发模式](https://blog.csdn.net/DabbyC/article/details/119998659)

