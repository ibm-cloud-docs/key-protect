---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: create standard encryption key, create secret, persist secret, create encryption key, standard encryption key API examples

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

# 创建标准密钥
{: #create-standard-keys}

可以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 来创建标准加密密钥，也可以使用 {{site.data.keyword.keymanagementserviceshort}} API 以编程方式来创建标准加密密钥。
{: shortdesc}

## 使用 GUI 创建标准密钥
{: #create-standard-key-gui}

[创建服务的实例后](/docs/services/key-protect?topic=key-protect-provision)，请完成以下步骤以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 来创建标准密钥。

1. [登录到 {{site.data.keyword.cloud_notm}} 控制台](https://{DomainName}/){: external}。
2. 转至**菜单** &gt; **资源列表**，以查看资源的列表。
3. 从 {{site.data.keyword.cloud_notm}} 资源列表中，选择您供应的 {{site.data.keyword.keymanagementserviceshort}} 实例。
4. 要创建新密钥，请单击**添加密钥**，然后选择**创建密钥**窗口。

    指定密钥的详细信息：

    <table>
      <tr>
        <th>设置</th>
        <th>描述</th>
      </tr>
      <tr>
        <td>名称</td>
        <td>
          <p>密钥的人类可读的唯一别名，以便可轻松识别密钥。</p>
          <p>为保护隐私，请确保密钥名称不包含个人可标识信息 (PII)，例如，姓名或位置。</p>
        </td>
      </tr>
      <tr></tr>
        <td>密钥类型</td>
        <td>要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect/concepts?topic=key-protect-envelope-encryption#key-types">密钥类型</a>。从密钥类型列表中，选择<b>标准密钥</b>。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 描述<b>创建密钥</b>设置</caption>
    </table>

5. 填写完密钥详细信息后，单击**创建密钥**以进行确认。 

## 使用 API 创建标准密钥
{: #create-standard-key-api}

通过对以下端点执行 `POST` 调用来创建标准密钥。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [检索服务和认证凭证以与服务中的密钥一起使用](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 使用以下 cURL 命令调用 [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect){: external}。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
      -d '{
     "metadata": {
"collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "extractable": <key_type>
       }
     ]
    }'
    ```
    {: codeblock}

    根据下表替换示例请求中的变量。
    <table>
      <tr>
        <th>变量</th>
        <th>描述</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>必需</strong>。区域缩写（例如，<code>us-south</code> 或 <code>eu-gb</code>），表示 {{site.data.keyword.keymanagementserviceshort}} 服务实例所在的地理区域。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">区域服务端点</a>。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>必需</strong>。您的 {{site.data.keyword.cloud_notm}} 访问令牌。在 cURL 请求中包含 <code>IAM</code> 令牌的完整内容，包括 Bearer 值。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">检索访问令牌</a>。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>必需</strong>。指定给您的 {{site.data.keyword.keymanagementserviceshort}} 服务实例的唯一标识。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">检索实例标识</a>。</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>用于跟踪和关联事务的唯一标识。</td>
      </tr>
      <tr>
        <td><varname>return_preference</varname></td>
        <td><p>标头，用于更改 <code>POST</code> 和 <code>DELETE</code> 操作的服务器行为。</p><p>将 <em>return_preference</em> 变量设置为 <code>return=minimal</code> 时，服务在响应的 entity-body 中仅返回密钥元数据，如密钥名称和标识值。在将变量设置为 <code>return=representation</code> 时，服务会返回密钥资料和密钥元数据。</p></td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>必需</strong>。密钥的人类可读的唯一名称，以便可轻松识别密钥。为保护隐私，请不要将个人数据存储为密钥的元数据。</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>密钥的扩展描述。为保护隐私，请不要将个人数据存储为密钥的元数据。</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>密钥在系统中到期的日期和时间（RFC 3339 格式）。如果省略了 <code>expirationDate</code> 属性，那么密钥不会到期。</td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>布尔值，用于确定密钥资料是否可以离开服务。</p>
          <p>将 <code>extractable</code> 属性设置为 <code>true</code> 时，服务会创建可存储在应用程序或服务中的标准密钥。</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 2. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 添加标准密钥所需的变量</caption>
    </table>

    为保护个人数据的机密性，在向服务添加密钥时，避免输入个人可标识信息 (PII)，例如，姓名或位置。有关 PII 的更多示例，请参阅 [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external} 的第 2.2 节。
    {: important}

    成功的 `POST api/v2/keys` 响应会返回密钥的标识值以及其他元数据。标识是指定给密钥的唯一标识，用于后续调用 {{site.data.keyword.keymanagementserviceshort}} API。

3. 可选：通过运行以下调用来获取 {{site.data.keyword.keymanagementserviceshort}} 服务实例中的密钥，以验证是否创建了密钥。

    ```cURL
    curl -X GET \
      https://us-south.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}


## 后续工作
{: #create-standard-key-next-steps}

- 要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档](https://{DomainName}/apidocs/key-protect){: external}。
