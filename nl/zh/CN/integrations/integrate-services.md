---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect integration, integrate service with Key Protect

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

# 集成服务
{: #integrate-services}

{{site.data.keyword.keymanagementservicefull}} 与数据和存储解决方案相集成，可帮助您在云中引入和管理自己的加密。
{: shortdesc}

[创建服务实例后](/docs/services/key-protect?topic=key-protect-provision)，您可以将 {{site.data.keyword.keymanagementserviceshort}} 与以下受支持的服务相集成：

<table>
    <tr>
        <th>服务</th>
        <th>描述</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>使用 {{site.data.keyword.keymanagementserviceshort}} 向存储区添加[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)。使用在 {{site.data.keyword.keymanagementserviceshort}} 中管理的根密钥来保护加密静态数据的数据加密密钥。要了解更多信息，请查看[与 {{site.data.keyword.cos_full_notm}} 集成](/docs/services/key-protect?topic=key-protect-integrate-cos)。</p>
        </td>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.containerlong_notm}}</p>
        </td>
        <td>
          <p>使用[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)来保护 {{site.data.keyword.containershort_notm}} 集群中的密钥。要了解更多信息，请查看[使用 {{site.data.keyword.keymanagementserviceshort}} 加密 Kubernetes 密钥](/docs/containers?topic=containers-encryption#keyprotect)。</p>
        </td>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.databases-for-postgresql_full_notm}}</p>
        </td>
        <td>
          <p>通过将根密钥与 {{site.data.keyword.databases-for-postgresql}} 部署相关联来保护数据库。要了解更多信息，请查看 [{{site.data.keyword.databases-for-postgresql}} 文档](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-key-protect)。</p>
        </td>
    </tr>
      <tr>
        <td>
          <p>{{site.data.keyword.cloud_notm}} 的 {{site.data.keyword.cloudant_short_notm}} ({{site.data.keyword.cloud_notm}} Dedicated)</p>
        </td>
        <td>
          <p>通过将根密钥与 {{site.data.keyword.cloudant_short_notm}} 专用硬件实例相关联来加强静态加密策略。要了解更多信息，请查看 [{{site.data.keyword.cloudant_short_notm}} 文档](/docs/services/Cloudant/offerings?topic=cloudant-security#secure-access-control)。</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">表 1. 描述适用于 {{site.data.keyword.keymanagementserviceshort}} 的集成</caption>
</table>

## 了解集成 
{: #understand-integration}

在将受支持的服务与 {{site.data.keyword.keymanagementserviceshort}} 相集成时，针对此服务启用[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)。此集成允许您使用在 {{site.data.keyword.keymanagementserviceshort}} 中存储的根密钥来打包用于加密静态数据的数据加密密钥。 

例如，您可以创建根密钥，在 {{site.data.keyword.keymanagementserviceshort}} 中管理密钥，并使用根密钥来保护跨不同云服务存储的数据。

![该图显示 {{site.data.keyword.keymanagementserviceshort}} 集成的上下文视图。](../images/kp-integrations_min.svg)

### {{site.data.keyword.keymanagementserviceshort}} API 方法
{: #envelope-encryption-api-methods}

在后台，{{site.data.keyword.keymanagementserviceshort}} API 驱动包络加密过程。  

下表列出用于添加或除去针对资源的包络加密的 API 方法。

<table>
  <tr>
    <th>方法</th>
    <th>描述</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-wrap-keys">打包（加密）数据加密密钥</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=unwrap</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-unwrap-keys">解包（解密）数据加密密钥</a></td>
  </tr>
  <caption style="caption-side:bottom;">表 2. 描述 {{site.data.keyword.keymanagementserviceshort}} API 方法</caption>
</table>

要了解有关在 {{site.data.keyword.keymanagementserviceshort}} 中以编程方式管理密钥的更多信息，请查看 [{{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
{: tip}

## 集成受支持的服务
{: #grant-access}

要添加集成，请使用 {{site.data.keyword.iamlong}} 仪表板在服务之间创建授权。授权将启用服务到服务访问策略，以便可以将云数据服务中的资源与在 {{site.data.keyword.keymanagementserviceshort}} 中管理的[根密钥](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types)相关联。

请确保在相同区域中供应两个服务，然后再创建授权。要了解有关服务授权的更多信息，请参阅[授予服务之间的访问权 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](/docs/iam?topic=iam-serviceauth){: new_window}。
{: note}

在准备好集成服务时，请使用以下步骤以创建授权：

1. 在菜单栏中，单击**管理** &gt; **安全性** &gt; **访问权 (IAM)**，然后选择**授权**。 
2. 单击**创建**。
3. 选择授权的源和目标服务。
 
  对于**源服务**，选择要与 {{site.data.keyword.keymanagementserviceshort}} 相集成的云数据服务。对于**目标服务**，选择 **{{site.data.keyword.keymanagementservicelong_notm}}**。

5. 启用**读取者**角色。

        借助_读取者_许可权，您的源服务可以浏览在指定的 {{site.data.keyword.keymanagementserviceshort}} 实例中供应的根密钥。


6. 单击**授权**。

## 后续工作
{: #integration-next-steps}

通过在 {{site.data.keyword.keymanagementserviceshort}} 中创建根密钥，向云资源添加高级加密。向受支持的云数据服务添加新资源，然后选择要用于高级加密的根密钥。

- 要了解有关使用 {{site.data.keyword.keymanagementserviceshort}} 服务创建根密钥的更多信息，请参阅[创建根密钥](/docs/services/key-protect?topic=key-protect-create-root-keys)。
- 要了解有关将自己的根密钥引入到 {{site.data.keyword.keymanagementserviceshort}} 服务的更多信息，请参阅[导入根密钥](/docs/services/key-protect?topic=key-protect-import-root-keys)。


