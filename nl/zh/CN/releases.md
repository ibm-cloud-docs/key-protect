---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: release notes, changelog, what's new, service updates, service bulletin

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 新增内容
{: #releases}

及时获取适用于 {{site.data.keyword.keymanagementservicefull}} 的新功能。 

## 2019 年 3 月
{: #mar-2019}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对基于策略的密钥轮换的支持
{: #added-support-policy-key-rotation}
最新更新日期：2019 年 3 月 22 日

现在可以使用 {{site.data.keyword.keymanagementserviceshort}} 为根密钥关联轮换策略。

有关更多信息，请参阅[设置轮换策略](/docs/services/key-protect?topic=key-protect-set-rotation-policy)。要了解有关 {{site.data.keyword.keymanagementserviceshort}} 中密钥轮换选项的更多信息，请查看[比较密钥轮换选项](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)。

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对传输密钥的 Beta 支持
最新更新日期：2019 年 3 月 20 日

通过为 {{site.data.keyword.keymanagementserviceshort}} 服务创建传输加密密钥，可将加密密钥安全导入到云。

有关更多信息，请参阅[将自己的加密密钥引入到云](/docs/services/key-protect?topic=key-protect-importing-keys)。

传输密钥当前为 Beta 功能。Beta 功能可以随时更改，并且未来的更新可能会引入与最新版本不兼容的更改。
{: important}

## 2019 年 2 月
{: #feb-2019}

### 更改：旧的 {{site.data.keyword.keymanagementserviceshort}} 服务实例
最新更新日期：2019 年 2 月 13 日

2017 年 12 月 15 日之前供应的 {{site.data.keyword.keymanagementserviceshort}} 服务实例在基于 Cloud Foundry 的旧基础架构上运行。这一旧 {{site.data.keyword.keymanagementserviceshort}} 服务将在 2019 年 5 月 15 日废弃。

**对您的意义**

如果您在较旧的 {{site.data.keyword.keymanagementserviceshort}} 服务实例中有激活的生产密钥，那么请确保在 2019 年 5 月 15 日之前将这些密钥迁移到新的服务实例中，以避免失去对加密数据的访问权。可以通过从 [{{site.data.keyword.cloud_notm}} 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/) 导航到资源列表来查看是否正在使用旧实例。如果 {{site.data.keyword.cloud_notm}} 资源列表的 **Cloud Foundry 服务**部分列有 {{site.data.keyword.keymanagementserviceshort}} 服务实例，或者如果正在使用 `https://ibm-key-protect.edge.bluemix.net` API 端点将服务的操作作为目标，那么您正在使用 {{site.data.keyword.keymanagementserviceshort}} 的旧实例。2019 年 5 月 15 日之后，您将无法再访问旧端点，并无法将该服务作为目标进行操作。

将加密密钥迁移到新服务实例时是否需要帮助？有关详细步骤，请查看 [GitHub 中的迁移客户端 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/kms-migration-client){: new_window}。如果您对密钥状态或迁移过程存在其他问题，请通过 [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com) 联系 Terry Mosbaugh。
{: tip}

## 2018 年 12 月
{: #dec-2018}

### 更改：{{site.data.keyword.keymanagementserviceshort}} API 端点
最新更新日期：2018 年 12 月 19 日

为与 {{site.data.keyword.cloud_notm}} 的新统一体验保持一致，{{site.data.keyword.keymanagementserviceshort}} 更新了其服务 API 的基本 URL。

现在可以更新应用程序来引用新的 `cloud.ibm.com` 端点。

- `keyprotect.us-south.bluemix.net` is now `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` is now `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` is now `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` is now `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` is now `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` is now `jp-tok.kms.cloud.ibm.com` 

此时每个区域服务端点的两种 URL 都受到支持。 

## 2018 年 10 月
{: #oct-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到东京区域
最新更新日期：2018 年 10 月 31 日

现在，您可以在东京区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect?topic=key-protect-regions)。

### 新增：{{site.data.keyword.keymanagementserviceshort}} CLI 插件
最新更新日期：2018 年 10 月 02 日

现在，可以使用 {{site.data.keyword.keymanagementserviceshort}} CLI 插件来管理 {{site.data.keyword.keymanagementserviceshort}} 服务实例中的密钥。

要了解如何安装该插件，请参阅[设置 CLI](/docs/services/key-protect?topic=key-protect-set-up-cli)。要了解有关 {{site.data.keyword.keymanagementserviceshort}} CLI 的更多信息，请查看 [CLI 参考文档](/docs/services/key-protect?topic=key-protect-cli-reference)。

## 2018 年 9 月
{: #sept-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对按需密钥轮换的支持
最新更新日期：2018 年 9 月 28 日

现在，可以使用 {{site.data.keyword.keymanagementserviceshort}} 按需轮换根密钥。

有关更多信息，请参阅[轮换密钥](/docs/services/key-protect?topic=key-protect-rotate-keys)。

### 新增：适用于云应用程序的端到端安全性教程
最新更新日期：2018 年 9 月 14 日

要查找代码样本来了解如何使用自己的加密密钥来加密存储区内容吗？

现在，可以遵循[新教程](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)来练习为云应用程序添加端到端安全性。

有关更多信息，请查看 [GitHub 中的样本应用程序 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}。

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到华盛顿区域
最新更新日期：2018 年 9 月 10 日

现在，您可以在华盛顿区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2018 年 8 月
{: #aug-2018}

### 更改：{{site.data.keyword.keymanagementserviceshort}} API 文档 URL
最新更新日期：2018 年 8 月 28 日

{{site.data.keyword.keymanagementserviceshort}} API 参考已移至新位置！ 

现在，可以从以下位置访问 API 文档：
https://{DomainName}/apidocs/key-protect. 

## 2018 年 3 月
{: #mar-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到法兰克福区域
最新更新日期：2018 年 3 月 21 日

现在，您可以在法兰克福区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2018 年 1 月
{: #jan-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到悉尼区域
最新更新日期：2018 年 1 月 31 日

现在，您可以在悉尼区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2017 年 12 月
{: #dec-2017}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对“自带密钥”(BYOK) 的支持
最新更新日期：2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} 现在支持“自带密钥”(BYOK) 和客户管理的加密。

- 引入了[根密钥](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types)（也称为“客户根密钥”(CRK)）作为服务中的主要资源。 
- 针对 {{site.data.keyword.cos_full_notm}} 存储区启用了[包络加密](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how)。

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到伦敦区域
最新更新日期：2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} 现在适用于伦敦区域。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect?topic=key-protect-regions)。

### 更改：{{site.data.keyword.iamshort}} 角色
最新更新日期：2017 年 12 月 15 日

更改了 {{site.data.keyword.iamshort}} 角色，该角色确定可对 {{site.data.keyword.keymanagementserviceshort}} 资源执行的操作。

- `管理者 (Administrator)` 现在是`管理者 (Manager)`
- `编辑者 (Editor)` 现在是`写入者 (Writer)`
- `查看者 (Viewer)` 现在是`读取者 (Reader)`

有关更多信息，请参阅[管理用户访问权](/docs/services/key-protect?topic=key-protect-manage-access)。

## 2017 年 9 月
{: #sept-2017}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对 Cloud IAM 的支持
最新更新日期：2017 年 9 月 19 日

现在，您可以使用 {{site.data.keyword.iamshort}} 来设置和管理 {{site.data.keyword.keymanagementserviceshort}} 资源访问策略。

有关更多信息，请参阅[管理用户访问权](/docs/services/key-protect?topic=key-protect-manage-access)。
