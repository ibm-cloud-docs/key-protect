---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# 故障诊断
{: #troubleshooting}

使用 {{site.data.keyword.keymanagementservicefull}} 的常见问题可能包括在与 API 交互时提供正确的头或凭证。在多种情况下，可通过执行几个简单步骤以解决这些问题。
{: shortdesc}

## 无法创建或删除密钥
{: #unable-to-create-keys}

访问 {{site.data.keyword.keymanagementserviceshort}} 用户界面时，看不到添加密钥或删除密钥选项。

从 {{site.data.keyword.cloud_notm}} 仪表板，选择 {{site.data.keyword.keymanagementserviceshort}} 服务的实例。
{: tsSymptoms}

您可以查看密钥列表，但是看不到用于添加或删除密钥的选项。 

您没有正确的权限来执行 {{site.data.keyword.keymanagementserviceshort}} 操作。
{: tsCauses} 

与您的管理员确认是否已在适用的资源组或服务实例中为您分配了正确的角色。有关角色的更多信息，请参阅[角色和许可权](/docs/services/key-protect?topic=key-protect-manage-access#roles)。
{: tsResolve}

## 获取帮助和支持
{: #getting-help}

如果您在使用 {{site.data.keyword.keymanagementserviceshort}} 时有任何疑问或遇到任何问题，可以检查 {{site.data.keyword.cloud_notm}}，或者在论坛中搜索相关信息或进行提问来获取帮助。还可以开具支持凭单。
{: shortdesc}

您可以通过转至[状态页面](https://{DomainName}/status?tags=platform,runtimes,services){: external}来检查 {{site.data.keyword.cloud_notm}} 是否可用。

可以查看论坛以了解是否有其他用户遇到相同问题。使用论坛进行提问时，请标记您的问题，以便 {{site.data.keyword.cloud_notm}} 开发团队可以看到。

- 如果有关于 {{site.data.keyword.keymanagementserviceshort}} 的技术问题，请将您的问题发布到 [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} 上，并使用“ibm-cloud”和“key-protect”标记问题。
- 有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external} 论坛。请包括“ibm-cloud”和“key-protect”标记。

有关使用论坛的更多详细信息，请参阅[获取支持](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external}。

有关提交 {{site.data.keyword.IBM_notm}} 支持凭单或支持级别和凭单严重性的更多信息，请参阅[联系支持人员](/docs/get-support?topic=get-support-getting-customer-support){: external}。
