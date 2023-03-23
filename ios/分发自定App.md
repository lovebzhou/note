- [分发自定 App](https://developer.apple.com/cn/custom-apps/)
- [Apple 商务管理](https://www.apple.com.cn/business/docs/site/Apple_Business_Manager_Getting_Started_Guide.pdf)
- [Apple 商务管理使用手册](https://support.apple.com/zh-cn/guide/apple-business-manager/welcome/web)
- [注册 Apple 商务管理](https://support.apple.com/zh-cn/guide/apple-business-manager/axm402206497/web)
- [Apple 商务管理的要求](https://support.apple.com/zh-cn/guide/apple-business-manager/axm6d9dc7acf/1/web/1)
- [iOS应用发布方式盘点+苹果商务详解](https://www.jianshu.com/p/c8361a83a338)


## 1. 概述

与商务和教育机构客户接洽，根据他们所在组织的独特需求，为其设计和构建自定 App。通过 Apple 商务管理和 Apple 校园教务管理，你不但能以安全私密的方式向特定的合作伙伴、客户和特许经营者进行分发，而且还能向内部员工分发专属 App。

### 创建自定 App

对于你专门为某个组织设计和开发的自定版本 App，你将负责维护代码并有保留知识产权的权利。

你可以提供：
-   适用于 iPhone、iPad、Mac 或 Apple TV 的 App
-   量身打造的外观和风格，例如公司徽标或品牌
-   针对业务流程或工作流的特定功能
-   针对 IT 环境的特殊配置
-   针对公司敏感或保密数据的安全功能
-   针对合作伙伴、客户、经销商，以及特许经营者的自定功能
-   针对企业内部员工的独特功能

### 运作方式

你在 App Store Connect 中指定的组织将能够看到你的 App，并在 Apple 商务管理和 Apple 校园教务管理的“App 和图书”部分中进行下载。你可以免费提供自定 App，也可以按你选择的任何价格等级收费。你只需在 App Store Connect 中指定允许下载你 App 的组织，并为 App 设置发布日期。

[进一步了解](https://developer.apple.com/cn/support/volume-purchase-and-custom-apps/)

### App Store Connect

通过 App Store Connect，你能够以非公开的方式向多达 69 个地区分发你的 App。上传你的 App 以供审核，并选择“分发自定 App”选项。如果 App 包含敏感数据，则需要向我们的审核团队提供示例数据及相关证明。确保你的税务和银行信息已妥当设置，以便 Apple 为你处理付款。你还可以在分发前邀请测试员对 App 进行 Beta 测试。

[了解如何使用 App Store Connect](https://developer.apple.com/cn/help/app-store-connect/manage-your-apps-availability/set-distribution-methods)

## 2. Apple 商务管理

组织可以注册为 Apple 商务用户，以使用 Apple 商务管理来购买和分发内容，以及自动完成设备部署。你指定的组织可以在 Apple 商务管理的“内容”部分中看到和购买你的 App，并能通过移动设备管理对其进行无缝分发。或者，组织可以选择向授权用户提供兑换码，供他们从 App Store 下载相应的 App。

[了解如何使用 Apple 商务管理](https://support.apple.com/zh-cn/guide/apple-business-manager/)

==由上可知，自定App的分发需要2个账号：苹果开发者账号、苹果商务管理账号。==

### 2.1. 注册 Apple 商务管理

https://support.apple.com/zh-cn/guide/apple-business-manager/axm402206497/1/web/1

#### 注册你的组织

- 组织信息：组织名称、邓白氏编码、电话号码、网站、时区和语言。
	- 网站URL：==须与管理式Apple ID域名相同==
- 申请人信息：姓名、工作电子邮箱、职务/职称。
	- 工作电子邮箱：==未与 App Store 或 iCloud 帐户关联，也未在其他任何 Apple 服务或网站上作为 Apple ID 使用的工作电子邮件地址。==请参阅[初始管理员帐户的电子邮件地址要求](https://support.apple.com/zh-cn/guide/apple-business-manager/axm6d9dc7acf/1/web/1#axm186482fc3)。
- 验证联系人：姓名、工作电子邮箱、职务/职称。

#### 确认注册和授予管理员访问权限

Apple 联系你的验证联系人并确认你的信息之后，该联系人将收到来自 Apple 商务管理的主题为“感谢你确认你的组织”的邮件。然后，该联系人即可完成以下流程。

1.  打开来自 Apple 商务管理的主题为“感谢你确认你的组织”的邮件。
2.  查看邮件并执行以下操作之一：
    -   点按“确认为[_人名_]”按钮，将此人设置为 Apple 商务管理的管理员。
        这是最初在 Apple 商务管理中注册的人的名字。
    -   如果你不希望将此人设置为管理员，请点按“另选他人”链接，输入其他人的信息，然后点按“提交”。
3.  你的验证联系人还必须勾选相应复选框，表明你批准此人代表你的组织负责签署 Apple 商务管理条款与条件。
    完成此流程后，被设为管理员的人将收到来自 Apple商务管理的主题为“注册完成”的邮件。

#### 完成注册流程

打开来自 Apple 商务管理的主题为“注册完成”的邮件，点按邮件中的“开始操作”按钮，填写该苹果商务管理账号的管理员信息：姓名、工作邮箱、密码、手机号码等信息。

> **注意**：因为这里需要新创建一个“管理式 Apple ID”，所以此处的邮箱不能填写提交申请时的 Apple ID 或者其他已经存在的 Apple ID，需要一个新的以公司网址为后缀的工作邮箱地址。

### 2.2. 在 Apple 商务管理中编辑偏好设置

#### 启用“自定 App”

自定 App 是由您或第三方开发者开发的 App，旨在满足组织的特定业务需求。请参阅[了解自定 App](https://support.apple.com/zh-cn/guide/apple-business-manager/axm58ba3112a/1/web/1)。

1.  在 Apple 商务管理中，使用具有管理员职务的用户登录。
2.  在边栏底部点按您的姓名，点按“偏好设置”，点按“注册信息”，然后点按“自定 App”旁边的“启用”。
3.  将您的 [组织 ID](https://support.apple.com/zh-cn/guide/apple-business-manager/aside/axm1f9788cbf/1/web/1)（位于“注册信息”窗口顶部）提供给 App 开发人员，以便其将您的组织添加到批准列表中。
4.  您现在可以从“自定 App”部分购买任何其他自定 App。自定 App 可使用兑换代码或管理式许可进行部署，并具有与 App Store 中的 App 相同的管理选项。