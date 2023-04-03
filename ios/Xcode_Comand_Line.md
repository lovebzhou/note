## 0. 概述

https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-WHAT_IS_THE_COMMAND_LINE_TOOLS_PACKAGE_

## 1. xcrun

xcrun是一个用于在命令行中运行Xcode相关工具的命令行实用程序。它的作用是在终端中查找和运行Xcode开发工具中的各种命令行工具。

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

## 4. xcodebuild

[Building from the Command Line with Xcode FAQ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html)

### 自动化生成、签名和导出应用程序

#### 0. 安装证书

下面命令可以安装证书，但需要手动输入密码。
```Shell
# 安装P12证书
security import certificate.p12 -k ~/Library/Keychains/login.keychain

# 安装p12即安装了开发证书的公钥和私钥，分发证书公钥需单独安装
securite import distribute.cer

# 安装描述文件
cp mobileprovision.mobileprovision ~/Library/MobileDevice/Provisioning\ Profiles/
```

#### 1. 生成Archive
```Shell
xcodebuild archive -workspace <workspace名字> -scheme <scheme名字> -archivePath <归档文件路径>

# 例：归档伙伴云
xcodebuild archive -workspace Huoban.xcworkspace -scheme Huoban -archivePath "~/Downloads/Huoban.xcarchive"
```
-   `<workspace名字>`：工作空间的名称
-   `<scheme名字>`：要归档的scheme名称
-   `<归档文件路径>`：指定归档文件的路径和名称。

#### 2.  签名&导出Archive
```Shell
xcodebuild -exportArchive -archivePath <归档文件路径> -exportOptionsPlist <导出选项Plist文件> -exportPath <导出文件路径>

# 例：签署伙伴云归档
xcodebuild -exportArchive -archivePath "~/Downloads/Huoban.xcarchive" -exportOptionsPlist "/Users/zhoubo/Downloads/Huoban 2023-03-17 12-21-34/ExportOptions.plist" -exportPath "~/Downloads/Huoban 2023-03-27 16-30-50"
```
-   `<归档文件路径>`：指定归档文件的路径和名称。
-   `<导出选项Plist文件>`：指定导出选项的plist文件：指定证书、描述文件、应用程序ID等信息。
-   `<导出文件路径>`：指定导出文件的路径和名称。

导出选项Plist文件的格式如下：
```Xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>compileBitcode</key>
	<false/>
	<key>destination</key>
	<string>export</string>
	<key>method</key>
	<string>ad-hoc</string>
	<key>provisioningProfiles</key>
	<dict>
		<key>com.huoban.cloudgrid</key>
		<string>match AdHoc com.huoban.cloudgrid</string>
		<key>com.huoban.cloudgrid.HuobanShare</key>
		<string>match AdHoc com.huoban.cloudgrid.HuobanShare</string>
	</dict>
	<key>signingCertificate</key>
	<string>Apple Distribution</string>
	<key>signingStyle</key>
	<string>manual</string>
	<key>stripSwiftSymbols</key>
	<true/>
	<key>teamID</key>
	<string>your_team_id</string>
</dict>
</plist>
```
- `<bundle_id>` 应用程序的bundle ID
- `<provisioning_profile_name>` 描述文件的名称

## security

```Shell
# 将描述文件导出为plist
security cms -D -i embedded.mobileprovision > result.plist open result.plist
```
