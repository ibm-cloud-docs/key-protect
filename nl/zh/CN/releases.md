---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

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

## 2018 年 10 月
{: #oct-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到亚太地区北部区域
最新更新日期：2018 年 10 月 31 日

现在，可以在亚太地区北部区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect/regions.html)。

### 新增：{{site.data.keyword.keymanagementserviceshort}} CLI 插件
最新更新日期：2018 年 10 月 02 日

现在，可以使用 {{site.data.keyword.keymanagementserviceshort}} CLI 插件来管理 {{site.data.keyword.keymanagementserviceshort}} 服务实例中的密钥。

要了解如何安装该插件，请参阅[设置 CLI](/docs/services/key-protect/set-up-cli.html)。要了解有关 {{site.data.keyword.keymanagementserviceshort}} CLI 的更多信息，请查看 [CLI 参考文档](/docs/services/key-protect/cli-reference.html)。

## 2018 年 9 月
{: #sept-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对按需密钥轮换的支持
最新更新日期：2018 年 9 月 28 日

现在，可以使用 {{site.data.keyword.keymanagementserviceshort}} 按需轮换根密钥。

有关更多信息，请参阅[轮换密钥](/docs/services/key-protect/rotate-keys.html)。

### 新增：适用于云应用程序的端到端安全性教程
最新更新日期：2018 年 9 月 14 日

要查找代码样本来了解如何使用自己的加密密钥来加密存储区内容吗？

现在，可以遵循[新教程](/docs/tutorials/cloud-e2e-security.html)来练习为云应用程序添加端到端安全性。

有关更多信息，请查看 [GitHub 中的样本应用程序 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}。

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到美国东部区域
最新更新日期：2018 年 9 月 10 日

现在，可以在美国东部区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect/regions.html)。

## 2018 年 8 月
{: #aug-2018}

### 更改：{{site.data.keyword.keymanagementserviceshort}} API 文档 URL
最新更新日期：2018 年 8 月 28 日

{{site.data.keyword.keymanagementserviceshort}} API 参考已移至新位置！ 

现在，可以从以下位置访问 API 文档：
https://console.bluemix.net/apidocs/key-protect

## 2018 年 3 月
{: #mar-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到法兰克福区域
最新更新日期：2018 年 3 月 21 日

现在，您可以在法兰克福区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect/regions.html)。

## 2018 年 1 月
{: #jan-2018}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到悉尼区域
最新更新日期：2018 年 1 月 31 日

现在，您可以在悉尼区域中创建 {{site.data.keyword.keymanagementserviceshort}} 资源。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect/regions.html)。

## 2017 年 12 月
{: #dec-2017}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对“自带密钥”(BYOK) 的支持
最新更新日期：2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} 现在支持“自带密钥”(BYOK) 和客户管理的加密。

- 引入了[根密钥](/docs/services/key-protect/concepts/envelope-encryption.html#key-types)（也称为“客户根密钥”(CRK)）作为服务中的主要资源。 
- 针对 {{site.data.keyword.cos_full_notm}} 存储区启用了[包络加密](/docs/services/key-protect/integrations/integrate-cos.html#kp-cos-how)。

### 新增：{{site.data.keyword.keymanagementserviceshort}} 扩展到伦敦区域
最新更新日期：2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} 现在适用于伦敦区域。 

有关更多信息，请参阅[区域和位置](/docs/services/key-protect/regions.html)。

### 更改：{{site.data.keyword.iamshort}} 角色
最新更新日期：2017 年 12 月 15 日

更改了 {{site.data.keyword.iamshort}} 角色，该角色确定可对 {{site.data.keyword.keymanagementserviceshort}} 资源执行的操作。

- `管理员 (Administrator)` 现在是`管理员 (Manager)`
- `编辑者 (Editor)` 现在是`作者 (Writer)`
- `查看者 (Viewer)` 现在是`读者 (Reader)`

有关更多信息，请参阅[管理用户访问权](/docs/services/key-protect/manage-access.html)。

## 2017 年 9 月
{: #sept-2017}

### 新增：{{site.data.keyword.keymanagementserviceshort}} 添加了对 Cloud IAM 的支持
最新更新日期：2017 年 9 月 19 日

现在，您可以使用 {{site.data.keyword.iamshort}} 来设置和管理 {{site.data.keyword.keymanagementserviceshort}} 资源访问策略。

有关更多信息，请参阅[管理用户访问权](/docs/services/key-protect/manage-access.html)。
