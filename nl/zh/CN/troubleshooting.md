---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 故障诊断
{: #troubleshooting}

使用 {{site.data.keyword.keymanagementservicefull}} 的常见问题可能包括在与 API 交互时提供正确的头或凭证。在多种情况下，可通过执行几个简单步骤以解决这些问题。
{: shortdesc}

## 无法访问用户界面
{: #unable-to-access-ui}

在访问 {{site.data.keyword.keymanagementserviceshort}} 用户界面时，不能按期望装入 UI。

从 {{site.data.keyword.cloud_notm}} 仪表板，选择 {{site.data.keyword.keymanagementserviceshort}} 服务的实例。
{: tsSymptoms}

您将看到以下错误： 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

2017 年 12 月 15 日，我们向 {{site.data.keyword.keymanagementserviceshort}} 服务添加了新功能，例如，[包络加密](/docs/services/key-protect/concepts/envelope-encryption.html)。现在，可全局供应 {{site.data.keyword.keymanagementserviceshort}} 服务，而无需指定 Cloud Foundry 组织和空间。
{: tsCauses}

这些更改影响较旧的服务实例的用户界面。如果在 2017 年 9 月 28 日之前创建 {{site.data.keyword.keymanagementserviceshort}} 实例，那么用户界面可能无法按期望工作。

我们正在修复此问题。作为临时解决方案，您可以继续使用 {{site.data.keyword.keymanagementserviceshort}} API 来管理密钥。
{: tsResolve}

您可以使用旧的 `https://ibm-key-protect.edge.bluemix.net` 端点来与 {{site.data.keyword.keymanagementserviceshort}} 服务进行交互。

**示例请求**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## 无法创建或删除密钥
{: #unable-to-create-keys}

访问 {{site.data.keyword.keymanagementserviceshort}} 用户界面时，看不到添加密钥或删除密钥选项。

从 {{site.data.keyword.cloud_notm}} 仪表板，选择 {{site.data.keyword.keymanagementserviceshort}} 服务的实例。
{: tsSymptoms}

您可以查看密钥列表，但是看不到用于添加或删除密钥的选项。 

您没有正确的权限来执行 {{site.data.keyword.keymanagementserviceshort}} 操作。
{: tsCauses} 

与您的管理员确认是否已在适用的资源组或服务实例中为您分配了正确的角色。有关角色的更多信息，请参阅[角色和许可权](/docs/services/key-protect/manage-access.html#roles)。
{: tsResolve}

## 获取帮助和支持
{: #getting-help}

如果您在使用 {{site.data.keyword.keymanagementserviceshort}} 时有任何疑问或遇到任何问题，可以检查 {{site.data.keyword.cloud_notm}}，或者在论坛中搜索相关信息或进行提问来获取帮助。还可以开具支持凭单。
{: shortdesc}

您可以通过转至[状态页面 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/status?tags=platform,runtimes,services)，来检查 {{site.data.keyword.cloud_notm}} 是否可用。

可以查看论坛以了解是否有其他用户遇到相同问题。使用论坛进行提问时，请标记您的问题，以便 {{site.data.keyword.cloud_notm}} 开发团队可以看到。

- 如果有关于 {{site.data.keyword.keymanagementserviceshort}} 的技术问题，请将您的问题发布到 [Stack Overflow ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} 上，并使用“ibm-cloud”和“key-protect”标记问题。
- 有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} 论坛。请包括“ibm-cloud”和“key-protect”标记。

有关使用论坛的更多详细信息，请参阅[获取帮助 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}。

有关提交 {{site.data.keyword.IBM_notm}} 支持凭单或支持级别和凭单严重性的更多信息，请参阅[联系支持人员 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}。
