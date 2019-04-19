---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# 设置 API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} 提供可与任何编程语言配合使用的 REST API，用于存储、检索和生成加密密钥。
{: shortdesc}

## 检索 {{site.data.keyword.cloud_notm}} 凭证
{: #retrieve-credentials}

要使用该 API，您需要生成您的服务和认证凭证。 

要收集凭证，请执行以下操作：

1. [生成 {{site.data.keyword.cloud_notm}} IAM 访问令牌](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. [检索用于唯一标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的实例标识](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID)。

## 构成 API 请求
{: #form-api-request}

对服务发出 API 调用时，请根据初始供应 {{site.data.keyword.keymanagementserviceshort}} 实例的方式来构造 API 请求。 

要构建请求，请将[区域服务端点](/docs/services/key-protect?topic=key-protect-regions)与相应的认证凭证配合使用。例如，如果为 `us-south` 区域创建了服务实例，请使用以下端点和 API 头浏览服务中的密钥：

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

将 `<access_token>` 和 `<instance_ID>` 替换为检索到的服务和认证凭证。

要跟踪 API 请求以防出错吗？如果在 cURL 请求中包含 `-v` 标志，您会在响应头中获得 `correlation-id` 值。可以使用此值来关联和跟踪请求以用于调试目的。
{: tip} 

## 后续工作
{: #set-up-api-next-steps}

您已准备好开始在 Key Protect 中管理加密密钥。要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
