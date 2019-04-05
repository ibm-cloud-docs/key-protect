---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

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
{:note: .note}
{:important: .important}

# 故障诊断
{: #troubleshooting}

使用 {{site.data.keyword.keymanagementservicefull}} 的常见问题可能包括在与 API 交互时提供正确的头或凭证。在多种情况下，可通过执行几个简单步骤以解决这些问题。
{: shortdesc}

## 无法删除我的 Cloud Foundry 服务实例
{: #unable-to-delete-service}

尝试删除 {{site.data.keyword.keymanagementserviceshort}} 服务实例时，未能按预期删除服务。

从 {{site.data.keyword.cloud_notm}} 仪表板中，导航至 **Cloud Foundry 服务**，然后选择 {{site.data.keyword.keymanagementserviceshort}} 的实例。单击 ⋮ 图标以打开服务产品的选项列表，然后单击**删除服务**。
{: tsSymptoms}

删除服务失败时，您会看到以下错误： 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

在 2017 年 12 月 15 日，{{site.data.keyword.keymanagementserviceshort}} 已从使用 Cloud Foundry 组织、空间和角色转为使用 IAM 和资源组。现在，可在资源组内供应 {{site.data.keyword.keymanagementserviceshort}} 服务，而无需指定 Cloud Foundry 组织和空间。
{: tsCauses}

这些更改对较旧服务实例的撤销供应有所影响。如果是 2017 年 9 月 28 日之前创建的 {{site.data.keyword.keymanagementserviceshort}} 实例，那么可能无法按预期删除服务。

要删除较旧的 {{site.data.keyword.keymanagementserviceshort}} 服务实例，必须先通过使用旧的 `https://ibm-key-protect.edge.bluemix.net` 端点与 {{site.data.keyword.keymanagementserviceshort}} 服务进行交互来删除现有密钥。
{: tsResolve}

要删除密钥和服务实例，请执行以下操作：

1. 通过 {{site.data.keyword.cloud_notm}} CLI 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: codeblock}

    **注：**如果登录失败，请运行 `bx login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。

2. 选择包含您 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 区域、组织和空间。

    请记下 CLI 输出中您的组织和空间的名称。您还可以运行 `ibmcloud cf target` 来设置目标 Cloud Foundry 组织和空间。

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. 检索 {{site.data.keyword.cloud_notm}} 组织和空间 GUID。

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
将 `<organization_name>` 和 `<space_name>` 替换为您指定给组织和空间的唯一别名。

4. 检索访问令牌。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. 通过运行以下 cURL 命令，列出服务实例中存储的密钥。

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    将 `<access_token>`, `<organization_GUID>` 和 `<space_GUID>` 替换为您在步骤 3 - 4 中检索到的值。 

6. 复制服务实例中存储的每个密钥的标识值。

7. 运行以下 cURL 命令以永久删除密钥及其内容。

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    将 `<access_token>`, `<organization_GUID>`, `<space_GUID>` 和 `<key_ID>` 替换为您在步骤 3 - 5 中检索到的值。针对每个密钥，重复运行该命令。    

8. 通过运行以下 cURL 命令，验证密钥是否已删除。

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    将 `<access_token>`, `<organization_GUID>` 和 `<space_GUID>` 替换为您在步骤 3 - 4 中检索到的值。 

9. 删除 {{site.data.keyword.keymanagementserviceshort}} 服务实例。

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. 可选：通过导航至 {{site.data.keyword.cloud_notm}} 仪表板，验证 {{site.data.keyword.keymanagementserviceshort}} 服务实例是否已删除。

    您还可以通过运行以下命令来列出目标空间中的可用 Cloud Foundry 服务。

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


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

2017 年 12 月 15 日，我们向 {{site.data.keyword.keymanagementserviceshort}} 服务添加了新功能，例如，[包络加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)。现在，可在资源组内供应 {{site.data.keyword.keymanagementserviceshort}} 服务，而无需指定 Cloud Foundry 组织和空间。
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

与您的管理员确认是否已在适用的资源组或服务实例中为您分配了正确的角色。有关角色的更多信息，请参阅[角色和许可权](/docs/services/key-protect?topic=key-protect-manage-access#roles)。
{: tsResolve}

## 获取帮助和支持
{: #getting-help}

如果您在使用 {{site.data.keyword.keymanagementserviceshort}} 时有任何疑问或遇到任何问题，可以检查 {{site.data.keyword.cloud_notm}}，或者在论坛中搜索相关信息或进行提问来获取帮助。还可以开具支持凭单。
{: shortdesc}

您可以通过转至[状态页面 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/status?tags=platform,runtimes,services)，来检查 {{site.data.keyword.cloud_notm}} 是否可用。

可以查看论坛以了解是否有其他用户遇到相同问题。使用论坛进行提问时，请标记您的问题，以便 {{site.data.keyword.cloud_notm}} 开发团队可以看到。

- 如果有关于 {{site.data.keyword.keymanagementserviceshort}} 的技术问题，请将您的问题发布到 [Stack Overflow ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} 上，并使用“ibm-cloud”和“key-protect”标记问题。
- 有关服务的问题和入门指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} 论坛。请包括“ibm-cloud”和“key-protect”标记。

有关使用论坛的更多详细信息，请参阅[获取支持 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/get-support?topic=get-support-using-avatar){: new_window}。

有关提交 {{site.data.keyword.IBM_notm}} 支持凭单或支持级别和凭单严重性的更多信息，请参阅[联系支持人员 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}。
