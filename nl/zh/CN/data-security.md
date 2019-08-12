---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 安全性与合规性
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} 已落实数据安全性策略来满足合规性需求，并确保数据在云中保持安全且受保护。
{: shortdesc}

## 安全性就绪性
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} 通过遵循针对系统、联网和安全工程的 {{site.data.keyword.IBM_notm}} 最佳实践来确保安全性就绪性。 

要了解有关 {{site.data.keyword.cloud_notm}} 上安全性控制的更多信息，请参阅[我如何知道自己的数据是安全的？](/docs/overview?topic=overview-security#security)。
{: tip}

### 数据加密
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} 使用 [{{site.data.keyword.cloud_notm}} 硬件安全模块 (HSM)](https://www.ibm.com/cloud/hardware-security-module){: external} 来生成受提供者管理的密钥资料，并执行[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)操作。HSM 是防篡改的硬件设备，用于存储并使用密钥资料，而不会将密钥公开到加密边界之外。

对服务的访问是通过 HTTPS 来实现的，并且内部 {{site.data.keyword.keymanagementserviceshort}} 通信使用传输层安全性 (TLS) 1.2 协议来加密正在传输的数据。

### 数据删除
{: #data-deletion}

删除密钥时，服务会将该密钥标记为已删除，并且该密钥会转变为_已销毁_状态。处于这一状态的密钥不再可恢复，并且使用该密钥的云服务无法再解密与该密钥关联的数据。数据会以加密形式保留在这些服务中。与密钥关联的元数据（例如，密钥的转换历史记录和名称）会保存在 {{site.data.keyword.keymanagementserviceshort}} 数据库中。 

删除 {{site.data.keyword.keymanagementserviceshort}} 中的密钥属于毁灭性操作。请记住，删除密钥之后，该操作将无法撤销，并且与此密钥关联的所有数据都会在删除该密钥时立即丢失。因此，在删除密钥之前，请查看与密钥关联的数据，并确保不再需要访问这些数据。请勿删除积极保护生产环境中数据的密钥。 

要帮助您确定哪些数据受密钥保护，可以通过运行 `ibmcloud iam authorization-policies` 来查看 {{site.data.keyword.keymanagementserviceshort}} 服务实例如何映射到现有云服务。要了解有关从控制台查看服务授权的更多信息，请参阅[授予服务之间的访问权](/docs/iam?topic=iam-serviceauth)。
{: note}

## 合规性就绪性
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} 满足全球、行业和区域合规性标准的控制，包括 GDPR、HIPAA 和 ISO 27001/27017/27018 及其他。 

有关 {{site.data.keyword.cloud_notm}} 合规性证书的完整列表，请参阅 [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}。
{: tip}

### 欧盟支持
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} 实施额外控制以保护您在欧盟 (EU) 的 {{site.data.keyword.keymanagementserviceshort}} 资源。 

如果使用德国法兰克福区域中的 {{site.data.keyword.keymanagementserviceshort}} 资源来处理欧洲居民的个人数据，那么可针对您的 {{site.data.keyword.cloud_notm}} 帐户启用“欧盟支持”设置。要查找更多信息，请参阅[启用欧盟支持设置](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)和[请求对欧盟中资源的支持](/docs/get-support?topic=get-support-getting-customer-support#eusupported)。

### 一般数据保护条例 (GDPR)
{: #gdpr}

GDPR 致力于在整个欧盟建立协调的数据保护法律框架，目的是让居民重获对其个人数据的控制权，同时针对在全球任何位置托管和处理这些数据的行为实施严格规则。

{{site.data.keyword.IBM_notm}} 致力于向客户和 {{site.data.keyword.IBM_notm}} 业务合作伙伴提供创新的数据隐私、安全和监管解决方案，以帮助他们最终符合 GDPR 就绪性的要求。

要确保 {{site.data.keyword.keymanagementserviceshort}} 资源实现 GDPR 合规性，请针对您的 {{site.data.keyword.cloud_notm}} 帐户[启用欧盟支持设置](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)。您可以通过查看以下附录了解有关 {{site.data.keyword.keymanagementserviceshort}} 如何处理和保护个人数据的更多信息。

- [{{site.data.keyword.keymanagementservicefull_notm}} 数据表附录 (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} 数据处理附录 (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA 支持
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} 满足《美国健康保险可移植性和责任法案》(HIPAA) 的控制以确保受保护健康信息 (PHI) 的安全。 

如果您或您的公司是 HIPAA 定义的受保护实体，那么可以针对您的 {{site.data.keyword.cloud_notm}} 帐户启用“HIPPA 支持”设置。要查找更多信息，请参阅[启用 HIPAA 支持设置](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa)。

### ISO 27001、27017 和 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} 通过了 ISO 27001、27017 和 27018 认证。您可以通过访问 [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external} 来查看合规性证书。 

### SOC 2 第 1 类
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} 已通过 SOC 2 第 1 类认证。有关请求 {{site.data.keyword.cloud_notm}} SOC 2 报告的信息，请参阅 [Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}。
