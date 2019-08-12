---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# 设置轮换策略
{: #set-rotation-policy}

可以通过使用 {{site.data.keyword.keymanagementservicefull}} 为根密钥设置自动轮换策略。
{: shortdesc}

为根密钥设置自动轮换策略时，将定期缩短密钥的生命周期，并限制受该密钥保护的信息量。

只能为 {{site.data.keyword.keymanagementserviceshort}} 中生成的根密钥创建轮换策略。如果最初导入的是根密钥，那么必须提供新的 Base64 编码的密钥资料，才能轮换密钥。有关更多信息，请参阅[根据需要轮换根密钥](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys)。
{: note}

要了解有关 {{site.data.keyword.keymanagementserviceshort}} 中密钥轮换选项的更多信息吗？请查看[比较密钥轮换选项](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)以获取更多信息。
{: tip}

## 在 GUI 中管理轮换策略
{: #manage-policies-gui}

如果想要使用图形界面来管理根密钥的策略，那么可以使用 {{site.data.keyword.keymanagementserviceshort}} GUI。

1. [登录到 {{site.data.keyword.cloud_notm}} 控制台](https://{DomainName}/){: external}。
2. 转至**菜单** &gt; **资源列表**，以查看资源的列表。
3. 从 {{site.data.keyword.cloud_notm}} 资源列表中，选择您供应的 {{site.data.keyword.keymanagementserviceshort}} 实例。
4. 在应用程序详细信息页面中，使用**密钥**表来浏览服务中的密钥。
5. 单击 ⋯ 图标以打开特定密钥的选项列表。
6. 在选项菜单中，单击**管理策略**来管理密钥的轮换策略。
7. 从轮换选项列表中，以月为单位选择轮换频率。

    如果密钥有现有轮换策略，那么界面会显示该密钥的现有轮换周期。

8. 单击**创建策略**以设置密钥的策略。

根据指定的轮换时间间隔，需要轮换密钥时，{{site.data.keyword.keymanagementserviceshort}} 会自动使用新密钥资料替换根密钥。

## 使用 API 管理轮换策略
{: #manage-rotation-policies-api}

### 查看轮换策略
{: #view-rotation-policy-api}

对于高级视图，可以通过对以下端点执行 `GET` 调用，浏览与根密钥关联的策略。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [检索服务和认证凭证](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 通过运行以下 cURL 命令，检索指定密钥的轮换策略。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    根据下表替换示例请求中的变量。
    <table>
      <tr>
        <th>变量</th>
        <th>描述</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必需</strong>。有现有轮换策略的根密钥的唯一标识。</td>
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
        <caption style="caption-side:bottom;">表 1. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 创建轮换策略所需的变量</caption>
    </table>

    成功的 `GET api/v2/keys/{id}/policies` 响应返回与密钥相关联的策略详细信息。以下 JSON 对象显示有现有轮换策略的根密钥的响应示例。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    `interval_month` 值以月为单位指示密钥轮换频率。

### 创建轮换策略
{: #create-rotation-policy-api}

通过对以下端点执行 `PUT` 调用来为根密钥创建轮换策略。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [检索服务和认证凭证](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 通过运行以下 cURL 命令，创建指定密钥的轮换策略。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
"collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必需</strong>。要为其创建轮换策略的根密钥的唯一标识。</td>
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
        <td><varname>rotation_interval</varname></td>
        <td><strong>必需</strong>。以月为单位来确定密钥轮换时间间隔的整数值。最小值为 <code>1</code>，最大值为 <code>12</code>。
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 1. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 创建轮换策略所需的变量</caption>
    </table>

    成功的 `PUT api/v2/keys/{id}/policies` 响应返回与密钥相关联的策略详细信息。以下 JSON 对象显示有现有轮换策略的根密钥的响应示例。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### 更新轮换策略
{: #update-rotation-policy-api}

通过对以下端点执行 `PUT` 调用来为根密钥更新现有策略。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [检索服务和认证凭证](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 通过运行以下 cURL 命令，替换指定密钥的轮换策略。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
"collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必需</strong>。要为其替换轮换策略的根密钥的唯一标识。</td>
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
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>必需</strong>。以月为单位来确定密钥轮换时间间隔的整数值。最小值为 <code>1</code>，最大值为 <code>12</code>。
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 1. 描述使用 {{site.data.keyword.keymanagementserviceshort}} API 创建轮换策略所需的变量</caption>
    </table>

    成功的 `PUT api/v2/keys/{id}/policies` 响应返回与密钥关联的已更新策略详细信息。以下 JSON 对象显示具有已更新轮换策略的根密钥的响应示例。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    已为密钥更新策略详细信息中的 `interval_month` 和 `updatedat` 值。如果不同用户更新了您最初创建的密钥的策略，那么 `updatedby` 值也会发生更改，以为发送请求的人员显示标识。
