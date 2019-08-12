---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# 创建传输密钥
{: #create-transport-keys}

可以通过先为 {{site.data.keyword.keymanagementserviceshort}} 服务实例创建传输加密密钥，支持将根密钥资料安全导入到云。
{: shortdesc}

传输密钥用于根据指定的策略加密根密钥资料，并将其安全导入到 {{site.data.keyword.keymanagementserviceshort}} 中。要了解有关将密钥安全导入到云的更多信息，请参阅[将加密密钥引入到云](/docs/services/key-protect/concepts?topic=key-protect-importing-keys)。

传输密钥当前为 Beta 功能。Beta 功能可以随时更改，并且未来的更新可能会引入与最新版本不兼容的更改。
{: important}

## 使用 API 创建传输密钥
{: #create-transport-key-api}

通过对以下端点执行 `POST` 调用来创建与 {{site.data.keyword.keymanagementserviceshort}} 服务实例关联的传输密钥。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [检索服务和认证凭证以与服务中的密钥一起使用](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 通过调用 [{{site.data.keyword.keymanagementserviceshort}} API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window} 为传输密钥设置策略。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
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
          <td><varname>expiration_time</varname></td>
          <td>
            <p>自传输密钥创建以来的时间（以秒为单位），用于确定密钥保持有效的时长。</p>
            <p>最小值为 300 秒（5 分钟），最大值为 86400 秒（24 小时）。缺省值为 600（10 分钟）。</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>在到期时间之内，不再可访问传输密钥之前，可检索到该传输密钥的次数。缺省值为 1。</td>
        </tr>
          <caption style="caption-side:bottom;">表 1. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 添加根密钥所需的变量</caption>
      </table>

    成功的 `POST api/v2/lockers` 请求为服务实例创建传输密钥，并返回其标识值以及其他元数据。标识是与传输密钥关联的唯一标识，用于对 {{site.data.keyword.keymanagementserviceshort}} API 的后续调用。

3. 可选：通过运行以下调用来检索有关服务实例的元数据，以验证是否创建了传输密钥。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 后续工作
{: #create-transport-key-next-steps}

- 要了解有关使用传输密钥将根密钥导入到服务中的更多信息，请查看[导入根密钥](/docs/services/key-protect?topic=key-protect-import-root-keys)。
- 要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
