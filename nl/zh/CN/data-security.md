---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: data security, Key Protect compliance, encryption key deletion

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

# 安全性与合规性
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} 已落实数据安全性策略来满足合规性需求，并确保数据在云中保持安全且受保护。
{: shortdesc}

## 数据安全性和加密
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} 使用 [{{site.data.keyword.cloud_notm}} 硬件安全模块 (HSM) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/cloud/hardware-security-module) 来生成受提供程序管理的密钥资料，并执行[包络加密](/docs/services/key-protect/envelope-encryption.html)操作。HSM 是防篡改的硬件设备，用于存储并使用密钥资料，而不会将密钥公开到加密边界之外。

对服务的访问是通过 HTTPS 来实现的，并且内部 {{site.data.keyword.keymanagementserviceshort}} 通信使用传输层安全性 (TLS) 1.2 协议来加密正在传输的数据。

要了解 {{site.data.keyword.keymanagementserviceshort}} 如何满足合规性需求的更多信息，请查看 [Platform 和服务合规性](/docs/overview/security.html#compliancetable)。

## 数据删除
{: #data-deletion}

删除密钥时，服务会将该密钥标记为已删除，并且该密钥会转变为_已销毁_状态。处于这一状态的密钥不再可恢复，并且使用该密钥的云服务无法再解密与该密钥关联的数据。数据会以加密形式保留在这些服务中。与密钥关联的元数据（例如，密钥的转换历史记录和名称）会保存在 {{site.data.keyword.keymanagementserviceshort}} 数据库中。 

删除 {{site.data.keyword.keymanagementserviceshort}} 中的密钥属于毁灭性操作。请记住，删除密钥之后，该操作将无法撤销，并且与此密钥关联的所有数据都会在删除该密钥时立即丢失。因此，在删除密钥之前，请查看与密钥关联的数据，并确保不再需要访问这些数据。请勿删除积极保护生产环境中数据的密钥。 

要帮助您确定哪些数据受密钥保护，可以通过运行 `ibmcloud iam authorization-policies` 来查看 {{site.data.keyword.keymanagementserviceshort}} 服务实例如何映射到现有云服务。要了解有关从控制台查看服务授权的更多信息，请参阅[授予服务之间的访问权](/docs/iam/authorizations.html#serviceauth)。
{: note}
