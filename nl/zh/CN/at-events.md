---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Activity Tracker 事件
{: #at-events}

作为安全主管、审计员或管理者，您可以使用 Activity Tracker 服务来跟踪用户和应用程序如何与 {{site.data.keyword.keymanagementservicefull}} 交互。
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker 记录由用户启动的会更改 {{site.data.keyword.cloud_notm}} 中服务状态的活动。您可以使用此服务来调查异常活动和关键操作，并满足法规审计要求。此外，您可以在操作发生时收到有关操作的警报。收集的事件符合 Cloud Auditing Data Federation (CADF) 标准。
 

{{site.data.keyword.cloud_notm}} 目录中当前提供两个 Activity Tracker 服务。{{site.data.keyword.keymanagementserviceshort}} 向这两个服务发送事件，并且您可以使用其中任一服务来监视 {{site.data.keyword.cloud_notm}} 中的 {{site.data.keyword.keymanagementserviceshort}} 活动。但是，{{site.data.keyword.cloudaccesstrailfull}} 已弃用且不能创建新实例，而且对现有服务实例的支持仅维持到 2019 年 9 月 30 日为止。

有关更多信息，请参阅：
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)（不推荐）

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## 事件列表
{: #at-actions}

下表列出生成事件的 {{site.data.keyword.keymanagementserviceshort}} 操作：

|操作|描述|
| ------------------------ | --------------------------- |
| `kms.secrets.create` |创建密钥|
| `kms.secrets.read`   |按标识检索密钥|
| `kms.secrets.delete` |按标识删除密钥|
| `kms.secrets.list`   |检索密钥列表|
| `kms.secrets.head`   |检索密钥数量|
| `kms.secrets.wrap`   |打包密钥|
| `kms.secrets.unwrap` |解包密钥|
| `kms.policies.read`  |查看密钥策略|
| `kms.policies.write` |设置密钥策略|
{: caption="表 1. 生成 Activity Tracker 事件的 {{site.data.keyword.keymanagementserviceshort}} 操作" caption-side="top"}

## 查看事件
{: #at-ui}

您可以使用 [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) 或 [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)（不推荐）来查看与 {{site.data.keyword.keymanagementserviceshort}} 服务实例相关联的 Activity Tracker 事件。

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### 使用 {{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

{{site.data.keyword.keymanagementserviceshort}} 实例生成的事件将自动转发到相同位置中可用的 {{site.data.keyword.at_full_notm}} 服务实例。 

{{site.data.keyword.at_full_notm}} 对于每个位置只能有一个实例。要查看事件，必须访问服务实例可用的相同位置中 {{site.data.keyword.at_full_notm}} 服务的 Web UI。有关更多信息，请参阅[通过 IBM Cloud UI 启动 Web UI](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2)。

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### 使用 {{site.data.keyword.cloudaccesstrailfull_notm}}（不推荐）
{: #at-ui-legacy}

{{site.data.keyword.cloudaccesstrailshort}} 事件在 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**中提供，该域在生成这些事件的 {{site.data.keyword.cloud_notm}} 区域中提供。有关更多信息，请参阅[查看事件](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4)。


## 分析事件
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

由于加密密钥的信息敏感度，当作为针对 {{site.data.keyword.keymanagementserviceshort}} 服务的 API 调用的结果生成事件时，生成的事件不包含有关密钥的详细信息。事件包含可用于在云环境中内部标识密钥的相关标识。相关标识是作为 `responseBody.content` 字段的一部分返回的字段。您可以使用此信息以将 {{site.data.keyword.keymanagementserviceshort}} 密钥与通过 {{site.data.keyword.cloudaccesstrailshort}} 事件报告的操作的信息相关联。
