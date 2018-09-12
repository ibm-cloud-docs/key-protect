---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.keymanagementserviceshort}} 入门

{{site.data.keyword.keymanagementservicefull}} 可帮助您为 {{site.data.keyword.cloud_notm}} 服务中的应用程序供应加密的密钥。本教程说明了如何使用 {{site.data.keyword.keymanagementserviceshort}} 仪表板来创建密钥和添加现有密钥，以便能够从一个中心位置管理数据加密。
{: shortdesc}

## 开始使用加密密钥
{: #get-started-keys}

在 {{site.data.keyword.keymanagementserviceshort}} 仪表板中，可以创建用于加密的新密钥，也可以导入现有密钥。 

从两种密钥类型中进行选择：

<dl>
  <dt>根密钥</dt>
    <dd>根密钥是可在 {{site.data.keyword.keymanagementserviceshort}} 中进行完全管理的对称密钥打包密钥。可以使用根密钥通过高级加密来保护其他密钥。要了解更多信息，请参阅<a href="/docs/services/key-protect/concepts/envelope-encryption.html">包络加密</a>。</dd>
  <dt>标准密钥</dt>
    <dd>标准密钥是用于加密的对称密钥。可以使用标准密钥直接对数据进行加密和解密。</dd>
</dl>

## 创建新密钥
{: #create-keys}

[创建 {{site.data.keyword.keymanagementserviceshort}} 的实例后](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps)，即可随时指定服务中的密钥。 

要创建第一个密钥，请完成以下步骤。 

1. 在 {{site.data.keyword.keymanagementserviceshort}} 仪表板中，单击**管理** &gt; **添加密钥**。
2. 要创建新密钥，请选择**生成新密钥**窗口。

    指定密钥的详细信息：

    <table>
      <tr>
        <th>设置</th>
        <th>描述</th>
      </tr>
      <tr>
        <td>名称</td>
        <td>
          <p>密钥的人类可以阅读的唯一别名，以便可轻松识别密钥。</p>
          <p>为保护隐私，请确保密钥名称不包含个人可标识信息 (PII)，例如，姓名或位置。</p>
        </td>
      </tr>
      <tr>
        <td>密钥类型</td>
        <td>要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">密钥类型</a>。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1.“生成新密钥”设置的描述</caption>
    </table>

3. 填写完密钥详细信息后，单击**生成密钥**以进行确认。 

服务中创建的密钥是 AES-GCM 算法支持的 256 位对称密钥。为了提高安全性，密钥通过位于安全 {{site.data.keyword.cloud_notm}} 数据中心且通过 FIPS 140-2 Level 2 认证的硬件安全模块 (HSM) 生成。 

## 添加现有密钥
{: #add-keys}

可以通过将现有密钥引入服务来利用“自带密钥”(BYOK) 的安全优势。 

要添加现有密钥，请完成以下步骤。

1. 在 {{site.data.keyword.keymanagementserviceshort}} 仪表板中，单击**管理** &gt; **添加密钥**。
2. 要上传现有密钥，请选择**输入现有密钥**窗口。

    指定密钥的详细信息：

    <table>
      <tr>
        <th>设置</th>
        <th>描述</th>
      </tr>
      <tr>
        <td>名称</td>
        <td>
          <p>密钥的人类可以阅读的唯一别名，以便可轻松识别密钥。</p>
          <p>为保护隐私，请确保密钥名称不包含个人可标识信息 (PII)，例如，姓名或位置。</p>
        </td>
      </tr>
      <tr>
        <td>密钥类型</td>
        <td>要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">密钥类型</a>。</td>
      </tr>
      <tr>
        <td>密钥资料</td>
        <td>要在 {{site.data.keyword.keymanagementserviceshort}} 服务中存储的密钥资料，例如，对称密钥。提供的密钥必须采用 base64 编码。</td>
      </tr>
      <caption style="caption-side:bottom;">表 2.“输入现有密钥”设置的描述</caption>
    </table>

3. 填写完密钥详细信息后，单击**添加新密钥**以进行确认。 

在 {{site.data.keyword.keymanagementserviceshort}} 仪表板中，可以检查新密钥的常规特征。 

## 后续工作

现在，可使用密钥对应用程序和服务进行编码。如果向服务添加了根密钥，那么可能要了解有关使用根密钥来保护用于加密静态数据的密钥的更多信息。请查看[打包密钥](/docs/services/key-protect/wrap-keys.html)以开始。

- 要查找有关使用根密钥来管理和保护加密密钥的更多信息，请查看[包络加密](/docs/services/key-protect/concepts/envelope-encryption.html)。
- 要了解有关将 {{site.data.keyword.keymanagementserviceshort}} 服务与其他云数据解决方案集成的更多信息，请[查看“集成”文档](/docs/services/key-protect/integrations/integrate-services.html)。
- 要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/apidocs/key-protect){: new_window}。
