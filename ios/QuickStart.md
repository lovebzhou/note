
## 1. 概述

开发、分发App需要创建`苹果开发者账户`。

分发自定App，需要组织注册为`Apple商务管理`用户，`苹果开发者`可以向指定的组织分发自定App。

### 1.1. 苹果开发者账户

Apple Developer 网站为你提供了所需的各种工具和信息，让你可以为 Apple 平台打造出色的 App。如果你是 Apple 平台开发的新手，可以考虑从免费功能开始着手。你只需[接受《Apple Developer 协议》](https://developer.apple.com/register/)，便可以创建帐户。使用此帐户可以下载 Beta 版软件和工具，访问论坛并报告错误。

当你准备好构建更多高级功能和分发 App 时，可以加入 Apple Developer Program 以在 App Store 上进行分发，或者加入 Apple Developer Enterprise Program 以在你的组织内分发内部专用 App。加入计划后，你便会在自己的帐户中看到与会员资格相关的其他选项。

[了解更多...](https://developer.apple.com/cn/help/account/)

### 1.2. Apple Developer Program 

加入 Apple Developer Program，通过 iPhone、iPad、Mac、Apple Watch 和 Apple TV版 App Store 向全球用户推出你的 App。会员资格包含开发和分发 App 所需的一切工具、资源和支持，包括获取 Beta 版软件、App 服务、测试工具以及 App 分析等。

[注册时所需的内容](https://developer.apple.com/cn/programs/enroll/)

### 1.3. Apple 商务管理

组织可以注册为 Apple 商务用户，以使用 Apple 商务管理来购买和分发内容，以及自动完成设备部署。你指定的组织可以在 Apple 商务管理的“内容”部分中看到和购买你的 App，并能通过移动设备管理对其进行无缝分发。或者，组织可以选择向授权用户提供兑换码，供他们从 App Store 下载相应的 App。

[了解如何使用 Apple 商务管理](https://support.apple.com/zh-cn/guide/apple-business-manager/)

### 2. 环境
#### XCode

https://developer.apple.com/xcode/

#### CocoaPods

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


### 参考资料

- [iOS 图标&启动图生成器](https://github.com/hxsxyz/QiAppIconGenerator)
- [iOS应用发布方式盘点+苹果商务详解](https://www.jianshu.com/p/c8361a83a338)
- [iOS商务管理分发模式](https://blog.csdn.net/DabbyC/article/details/119998659)

