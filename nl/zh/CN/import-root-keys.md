---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# 导入根密钥
{: #import-root-keys}

可以使用 {{site.data.keyword.keymanagementservicefull}} 通过 {{site.data.keyword.keymanagementserviceshort}} GUI 来确保现有根密钥的安全，也可以使用 {{site.data.keyword.keymanagementserviceshort}} API 以编程方式来确保现有根密钥的安全。
{: shortdesc}

根密钥是用于保护云中已加密数据的安全性的对称密钥打包密钥。有关将根密钥导入到 {{site.data.keyword.keymanagementserviceshort}} 之中的更多信息，请参阅[将自己的加密密钥引入到云](/docs/services/key-protect?topic=key-protect-importing-keys)。

## 使用 GUI 导入根密钥
{: #import-root-key-gui}

[创建服务的实例后](/docs/services/key-protect?topic=key-protect-provision)，请完成以下步骤以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 来添加现有根密钥。

1. [登录到 {{site.data.keyword.cloud_notm}} 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/){: new_window}。
2. 转至**菜单** &gt; **资源列表**，以查看资源的列表。
3. 从 {{site.data.keyword.cloud_notm}} 资源列表中，选择您供应的 {{site.data.keyword.keymanagementserviceshort}} 实例。
4. 要导入密钥，请单击**添加密钥**，然后选择**导入自己的密钥**窗口。

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
      <tr>
        <td>密钥类型</td>
        <td>要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">密钥类型</a>。从密钥类型列表中，选择<b>根密钥</b>。</td>
      </tr>
      <tr>
        <td>密钥资料</td>
        <td>
          <p>要在服务中存储和管理的 Base64 编码的密钥资料，例如现有密钥打包密钥。</p>
          <p>确保密钥资料满足以下需求：</p>
          <p>
            <ul>
              <li>密钥必须为 128 位、192 位或 256 位。</li>
              <li>数据字节（例如，对于 256 位，为 32 个字节）必须使用 Base64 编码进行编码。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 描述<b>导入自己的密钥</b>设置</caption>
    </table>

5. 填写完密钥详细信息后，单击**导入密钥**以进行确认。 

## 使用 API 导入根密钥
{: #import-root-key-api}

可以使用 {{site.data.keyword.keymanagementserviceshort}} API 将根密钥导入到服务中。

通过[查看创建和加密密钥资料的选项](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)提前计划导入密钥。为了提高安全性，可以在将密钥资料引入到云之前，通过使用[传输密钥](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys)加密密钥资料来支持安全导入这些密钥资料。如果想要在不使用传输密钥的情况下导入根密钥，请跳至[步骤 4](#import-root-key)。
{: note}

### 步骤 1：创建传输密钥
{: #create-transport-key}

传输密钥当前为 Beta 功能。Beta 功能可以随时更改，并且未来的更新可能会引入与最新版本不兼容的更改。
{: important}

通过对以下端点执行 `POST` 调用来为服务实例创建传输密钥。

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
      <td><strong>必需</strong>。区域缩写（例如，<code>us-south</code> 或 <code>eu-gb</code>），表示 {{site.data.keyword.keymanagementserviceshort}} 服务实例所在的地理区域。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">区域服务端点</a>。</td>
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
      <caption style="caption-side:bottom;">表 2. 描述创建传输密钥 {{site.data.keyword.keymanagementserviceshort}} API 所需的变量</caption>
  </table>

  成功的 `POST api/v2/lockers` 响应会返回传输密钥的标识值以及其他元数据。标识是与传输密钥关联的唯一标识，用于对 {{site.data.keyword.keymanagementserviceshort}} API 的后续调用。

### 步骤 2：检索传输密钥和导入令牌
{: #retrieve-transport-key}

通过对以下端点执行 `GET` 调用来检索传输密钥和导入令牌。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. 使用以下 cURL 命令调用 [{{site.data.keyword.keymanagementserviceshort}} API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><strong>必需</strong>。区域缩写（例如，<code>us-south</code> 或 <code>eu-gb</code>），表示 {{site.data.keyword.keymanagementserviceshort}} 服务实例所在的地理区域。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">区域服务端点</a>。</td>
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
        <td><varname>locker_ID</varname></td>
        <td><strong>必需</strong>。您在<a href="#create-transport-key">步骤 1</a> 中创建的传输密钥的标识。</td>
      </tr>
        <caption style="caption-side:bottom;">表 3. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 检索传输密钥所需的变量</caption>
    </table>

    成功的 `GET api/v2/lockers/{id}` 响应返回 4096 位 PKIX 格式的 DER 编码公共加密密钥，您可以使用该密钥来加密根密钥资料以及用于验证传输密钥完整性的导入令牌。

### 步骤 3：加密密钥资料
{: #encrypt-root-key}

检索传输密钥之后，可以使用该密钥来加密要导入到 {{site.data.keyword.keymanagementserviceshort}} 之中的密钥资料。  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

要在内部部署中生成密钥资料，请[查看创建对称加密密钥的选项](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)。例如，您可能想要使用组织的内部密钥管理系统（受内部部署硬件安全模块 (HSM) 支持）创建并导出密钥资料。
{: note}

要加密密钥资料：

1. 从内部部署密钥管理系统中以二进制格式导出 256 位密钥资料。

    要了解如何创建并导出密钥资料，请参阅有关内部部署 HSM 或密钥管理系统的文档。

2. 使用步骤 2 中[已检索到的传输密钥](#retrieve-transport-key)来加密密钥资料。

   加密密钥资料时，请使用 `RSAES_OAEP_SHA_256` 加密方案。这是 {{site.data.keyword.keymanagementserviceshort}} 用于创建传输密钥的缺省方案。在 {{site.data.keyword.keymanagementserviceshort}} 中为了避免解密问题，针对密钥资料运行 RSAES_OAEP 加密时，请不要包含可选的 `label` 参数。要了解如何针对密钥资料运行 RSA 加密，请参阅有关内部部署 HSM 或密钥管理系统的文档。

3. 继续执行下一步之前，请确保已加密的密钥资料采用 Base64 编码。

### 步骤 4：导入密钥资料
{: #import-root-key}

[加密密钥资料，并为其采用 Base64 编码之后](#encrypt-root-key)，可以通过对以下端点执行 `POST` 调用来将根密钥导入到 {{site.data.keyword.keymanagementserviceshort}} 中。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. 使用以下 cURL 命令调用 [{{site.data.keyword.keymanagementserviceshort}} API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
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
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
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
        <td><strong>必需</strong>。区域缩写（例如，<code>us-south</code> 或 <code>eu-gb</code>），表示 {{site.data.keyword.keymanagementserviceshort}} 服务实例所在的地理区域。有关更多信息，请参阅<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">区域服务端点</a>。</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>要在服务中存储和管理的 Base64 编码的密钥资料，例如现有密钥打包密钥。</p>
          <p>确保密钥资料满足以下需求：</p>
          <p>
            <ul>
              <li>密钥必须为 128 位、192 位或 256 位。</li>
              <li>数据字节（例如，对于 256 位，为 32 个字节）必须使用 Base64 编码进行编码。</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>布尔值，用于确定密钥资料是否可以离开服务。</p>
          <p>将 <code>extractable</code> 属性设置为 <code>false</code> 时，服务会将密钥指定为可用于 <code>wrap</code> 或 <code>unwrap</code> 操作的根密钥。</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>用于<a href="#encrypt-root-key">加密密钥资料</a>的加密方案。当前支持 <code>RSAES_OAEP_SHA_256</code>。要在不使用传输密钥和导入令牌的情况下导入根密钥资料，请省略 <code>encryption_algorithm</code> 属性。</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>用于验证传输密钥的活动性和完整性的导入令牌。使用传输密钥来加密密钥资料时，必须提供<a href="#retrieve-transport-key">步骤 2</a> 中检索到的同一导入令牌。要在不使用传输密钥和导入令牌的情况下导入根密钥资料，请省略 <code>importToken</code> 属性。</td>
      </tr>
        <caption style="caption-side:bottom;">表 4. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 添加根密钥所需的变量</caption>
    </table>

    为保护个人数据的机密性，在向服务添加密钥时，避免输入个人可标识信息 (PII)，例如，姓名或位置。有关 PII 的更多示例，请参阅 [NIST Special Publication 800-122 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window} 的第 2.2 节。
    {: important}

    成功的 `POST api/v2/keys` 响应会返回密钥的标识值以及其他元数据。标识是指定给密钥的唯一标识，用于后续调用 {{site.data.keyword.keymanagementserviceshort}} API。

2. 可选：通过运行以下调用来浏览 {{site.data.keyword.keymanagementserviceshort}} 服务实例中的密钥，以验证是否添加了密钥。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 后续工作
{: #import-root-key-next-steps}

- 要了解有关使用包络加密保护密钥的更多信息，请查看[打包密钥](/docs/services/key-protect?topic=key-protect-wrap-keys)。
- 要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
