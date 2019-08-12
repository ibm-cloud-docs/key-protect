---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# 常见问题
{: #faqs}

您可以使用以下常见问题来帮助使用 {{site.data.keyword.keymanagementservicelong}}。

## {{site.data.keyword.keymanagementserviceshort}} 如何定价？
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} 提供[累进层套餐](https://{DomainName}/catalog/services/key-protect)，并对需要 20 个或更少密钥的用户免费。每个 {{site.data.keyword.cloud_notm}} 帐户可具有 20 个免费密钥。如果您的团队需要多个 {{site.data.keyword.keymanagementserviceshort}} 实例，那么 {{site.data.keyword.cloud_notm}} 会为该帐户下的所有实例添加激活的密钥，然后再应用定价。 

## 激活的加密密钥是什么？
{: #what-is-active-encryption-key}
{: faq}

将加密密钥导入到 {{site.data.keyword.keymanagementserviceshort}} 中时，或使用 {{site.data.keyword.keymanagementserviceshort}} 从其 HSM 中生成密钥时，这些密钥即成为_激活的_密钥。定价取决于 {{site.data.keyword.cloud_notm}} 帐户中所有已激活密钥的个数。 

## 如何分组和管理密钥？
{: #how-to-group-keys}
{: faq}

从定价的角度来看，使用 {{site.data.keyword.keymanagementserviceshort}} 的最佳方法是先创建有限数量的根密钥，然后再使用这些根密钥来加密由外部应用程序或云数据服务创建的数据加密密钥。 

要了解有关使用根密钥保护数据加密密钥的更多信息，请查看[使用包络加密保护数据](/docs/services/key-protect?topic=key-protect-envelope-encryption)。

## 根密钥是什么？
{: #what-is-root-key}
{: faq}

根密钥是 {{site.data.keyword.keymanagementserviceshort}} 中的主要资源。根密钥是对称密钥打包密钥，用作使用[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)对数据服务中所存储的其他密钥进行保护的信任根。通过 {{site.data.keyword.keymanagementserviceshort}}，可以创建、存储和管理根密钥的生命周期，从而完全控制存储在云中的其他密钥。 

## 包络加密是什么？
{: #what-is-envelope-encryption}
{: faq}

包络加密是指先使用_数据加密密钥_对数据进行加密，然后再使用高度安全的_密钥合并密钥_对数据加密密钥进行加密的做法。
您的数据通过应用多层加密来进行静态保护。要了解如何为 {{site.data.keyword.cloud_notm}} 资源启用包络加密，请查看[集成服务](/docs/services/key-protect?topic=key-protect-integrate-services)。

## 密钥名称的长度可以为多少？
{: #key-names}
{: faq}

可以使用长度不超过 90 个字符的密钥名称。

## 是否可以将个人信息存储为密钥的元数据？
{: #personal-data}
{: faq}

为保护个人数据的机密性，请不要将个人可标识信息 (PII) 存储为密钥的元数据。个人信息包括您的姓名、地址、电话号码或电子邮件地址，或者包括可能识别、联系或查找到您、您的客户或其他任何人的其他信息。


对于存储为 {{site.data.keyword.keymanagementserviceshort}} 资源和加密密钥的元数据的任何信息，您负责确保其安全性。有关个人数据的更多示例，请参阅 [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external} 的第 2.2 节。
{: important}

## 在一个区域创建的密钥可以在另一个区域使用吗？
{: #keys-across-regions}
{: faq}

加密密钥仅限在它们创建所在的区域使用。{{site.data.keyword.keymanagementserviceshort}} 不会复制加密密钥或将其导出到其它区域。

## 如何控制谁有权访问密钥？
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} 支持由 {{site.data.keyword.iamlong}} 管理的集中访问控制系统来帮助您管理加密密钥的用户和访问权。如果您是服务的安全管理员，那么可以为团队成员分配与要授予的[特定 {{site.data.keyword.keymanagementserviceshort}} 许可权相对应的 Cloud IAM 角色](/docs/services/key-protect?topic=key-protect-manage-access#roles)。

## 如何监视对 {{site.data.keyword.keymanagementserviceshort}} 执行的 API 调用？
{: faq}

可以使用 {{site.data.keyword.cloudaccesstrailfull_notm}} 服务来跟踪用户和应用程序如何与 {{site.data.keyword.keymanagementserviceshort}} 服务实例交互。例如，在 {{site.data.keyword.keymanagementserviceshort}} 中创建、导入、删除或读取密钥时，将生成 {{site.data.keyword.cloudaccesstrailshort}} 事件。这些事件将自动转发至在供应 {{site.data.keyword.keymanagementserviceshort}} 服务的区域中的 {{site.data.keyword.cloudaccesstrailshort}} 服务。

要了解更多信息，请查看 [Activity Tracker 事件](/docs/services/key-protect?topic=key-protect-at-events)。

## 删除密钥时会发生什么？
{: #key-destruction}
{: faq}

删除密钥时，服务会将该密钥标记为已删除，并且该密钥会转变为_已销毁_状态。处于这一状态的密钥不再可恢复，并且使用该密钥的云服务无法再解密与该密钥关联的数据。数据会以加密形式保留在这些服务中。与密钥关联的元数据（例如，密钥的转换历史记录和名称）会保存在 {{site.data.keyword.keymanagementserviceshort}} 数据库中。 

在删除密钥之前，确保不再需要访问与密钥相关联的任何数据。此操作无法撤销。

## 需要撤销服务实例时会发生什么？
{: #deprovision-service}
{: faq}

如果您决定离开 {{site.data.keyword.keymanagementserviceshort}} 并继续下一步，那么必须从服务实例中删除其余所有密钥才能撤销该服务。删除服务实例之后，{{site.data.keyword.keymanagementserviceshort}} 会使用[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)对与服务实例相关联的所有数据进行碎片化加密处理。 

