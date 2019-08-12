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

身為安全性管理者、審核員或管理員，您可以使用 Activity Tracker 服務來追蹤使用者及應用程式如何與 {{site.data.keyword.keymanagementservicefull}} 的互動。
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker 會記錄由使用者起始且會變更 {{site.data.keyword.cloud_notm}} 中服務狀態的活動。您可以使用此服務來調查異常活動與重要動作以符合法規審核需求。此外，發生狀況時會警示您採取動作。收集的事件符合 Cloud Auditing Data Federation (CADF) 標準。 

{{site.data.keyword.cloud_notm}} 型錄中目前有 2 個 Activity Tracker 服務可供使用。{{site.data.keyword.keymanagementserviceshort}} 會將事件傳送至這兩個服務，您可以使用其中一個服務來監視 {{site.data.keyword.cloud_notm}} 中的 {{site.data.keyword.keymanagementserviceshort}} 活動。不過，{{site.data.keyword.cloudaccesstrailfull}} 已淘汰，因此無法建立新的實例，僅在 2019 年 9 月 30 日之前提供現有服務實例的支援。

如需相關資訊，請參閱：
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)（已淘汰）

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## 事件清單
{: #at-actions}

下表會列出產生事件的 {{site.data.keyword.keymanagementserviceshort}} 動作：

|動作|說明|
| ------------------------ | --------------------------- |
| `kms.secrets.create` |建立金鑰|
| `kms.secrets.read`   |依 ID 擷取金鑰|
| `kms.secrets.delete` |依 ID 刪除金鑰|
| `kms.secrets.list`   |擷取金鑰清單|
| `kms.secrets.head`   |擷取金鑰數目|
| `kms.secrets.wrap`   |包裝金鑰|
| `kms.secrets.unwrap` |解除包裝金鑰|
| `kms.policies.read`  |檢視金鑰原則|
| `kms.policies.write` |設定金鑰原則|
{: caption="表 1. 產生 Activity Tracker 的 {{site.data.keyword.keymanagementserviceshort}} 動作" caption-side="top"}

## 檢視事件
{: #at-ui}

透過使用 [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) 或 [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)（已淘汰），您可以檢視與 {{site.data.keyword.keymanagementserviceshort}} 服務實例相關聯的 Activity Tracker 事件。

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### 使用 {{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

由 {{site.data.keyword.keymanagementserviceshort}} 的實例所產生的事件會自動轉遞至相同位置中可以使用的 {{site.data.keyword.at_full_notm}} 服務實例。 

{{site.data.keyword.at_full_notm}} 在每個位置只能有一個實例。若要檢視事件，您必須在可以使用服務實例的相同位置中存取 {{site.data.keyword.at_full_notm}} 服務的 Web 使用者介面。如需相關資訊，請參閱[透過 IBM Cloud 使用者介面來啟動 Web 使用者介面](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2)。

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### 使用 {{site.data.keyword.cloudaccesstrailfull_notm}}（已淘汰）
{: #at-ui-legacy}

{{site.data.keyword.cloudaccesstrailshort}} 事件提供於 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**，這提供於產生事件的 {{site.data.keyword.cloud_notm}} 地區。如需相關資訊，請參閱[檢視事件](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4)。


## 分析事件
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

由於加密金鑰的資訊機密性，當因為對 {{site.data.keyword.keymanagementserviceshort}} 服務的 API 呼叫而產生事件時，所產生的事件不會包含金鑰的相關詳細資訊。事件包含了一個相關性 ID，您可以用來在雲端環境中，在內部識別該金鑰。相關性 ID 是一個欄位，它是作為 `responseBody.content` 欄位的一部分傳回。您可以使用此資訊讓 {{site.data.keyword.keymanagementserviceshort}} 金鑰與透過 {{site.data.keyword.cloudaccesstrailshort}} 事件報告之動作的資訊產生關聯。
