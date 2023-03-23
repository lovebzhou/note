## 0. 概述

https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-WHAT_IS_THE_COMMAND_LINE_TOOLS_PACKAGE_

## 1. xcrun

Find and execute the named command line tool from the active developer directory.
查找并执行active开发者目录中的CLI命令。

The active developer directory can be set using `xcode-select`, or via the DEVELOPER_DIR environment variable. 
Active开发则目录可通过`xcod-select`设置，或通过环境变量`DEVELOPER_DIR`。

xcrun provides a means to locate or invoke developer tools from the command-line, without requiring users to modify Makefiles or otherwise take inconvenient measures to support multiple Xcode tool chains.
xcrun提供了从命令行定位、调用开发者工具的途径——它不需要开发者通过修改Makefile或其它复杂操作来支持多个XCode工具链。

```Shell
# 查看xcrun帮助手册
man xcrun
```

## 2. simctl

Command line utility to control the Simulator
模拟器控制命令

For subcommands that require a `device` argument, you may specify a device UDID
or the special "booted" string which will cause simctl to pick a booted device.
子命令需要`device`参数，你可以指定一个设备UDID或指定”引导“字符串让simctl来启动一个设备。

```Shell
# 查看subcommand帮助
xcrun simctl help [subcommand]
```

- boot: Boot a device or device pair.
- launch: Launch an application by identifier on a device.
- list:  List available devices, device types, runtimes, or device pairs.
- push: Send a simulated push notification.
- install: Install an app on a device.

### list
List available devices, device types, runtimes, or device pairs.
```Shell
# 列出iOS版本为16.2的模拟设备
xcrun simctl list devices "16.2"

# 列出全部可用模拟设备
xcrun simctl list devices available
# == Devices ==
# -- iOS 16.2 --
#  iPhone 14 (3B2E17FA-0C3C-4F84-8886-B8447498DE40) (Booted) 
#  iPhone 14 Plus (17820D5F-D185-4AF8-92E3-205FBFE709A7) (Shutdown)
#  iPhone 14 Pro (AF41D032-52CB-41D6-ADFC-494DEFDBA3E6) (Shutdown)
# ...

```

### boot
Boot a device or device pair.
```Shell
xcrun simctl boot 17820D5F-D185-4AF8-92E3-205FBFE709A7
```

### launch
Launch an application by identifier on a device.
```Shell
xcrun simctl launch 3B2E17FA-0C3C-4F84-8886-B8447498DE40 com.huoban.cloudgrid
```

### push
Send a simulated push notification
```Shell
xcrun simctl push 3B2E17FA-0C3C-4F84-8886-B8447498DE40 com.huoban.cloudgrid payload.json
```
payload.json
```JSON
{
    "aps" : {
        "alert" : {
            "title" : "sarunw.com",
            "body" : "A weekly blog about iOS development"
        },
        "badge" : 5
    }
}
```

## 3. xctrace

Record, import, export and symbolicate Instruments .trace files.

```Shell
# 列出可用设备
xctrace list devices
```

## xcodebuild

build Xcode projects and workspaces

- App构建
```
xcodebuild -workspace ./ios/HuobanRN.xcworkspace -configuration Debug -scheme HuobanRN -destination id=3B2E17FA-0C3C-4F84-8886-B8447498DE40
```

## security

```Shell
security cms -D -i embedded.mobileprovision > result.plist open result.plist
```
