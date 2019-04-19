---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #activity-tracker-events}

請使用 {{site.data.keyword.cloudaccesstrailfull}} 服務來追蹤使用者及應用程式與 {{site.data.keyword.keymanagementservicefull}} 的互動情況。
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} 服務會記錄由使用者起始，且會變更 {{site.data.keyword.cloud_notm}} 中服務狀態的活動。 

如需相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}} 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started){: new_window}。

## 事件清單
{: #list-activity-tracker-events}

下表列出會產生事件的動作：

|動作|說明|
| -------------------- | --------------------------- |
| `kms.secrets.create` |建立金鑰|
| `kms.secrets.read`   |依 ID 擷取金鑰|
| `kms.secrets.delete` |依 ID 刪除金鑰|
| `kms.secrets.list`   |擷取金鑰清單|
| `kms.secrets.head`   |擷取金鑰數目|
| `kms.secrets.wrap`   |包裝金鑰|
| `kms.secrets.unwrap` |解除包裝金鑰|
| `kms.policies.read`  |檢視金鑰原則|
| `kms.policies.write` |設定金鑰原則|
{: caption="表 1. 產生 {{site.data.keyword.cloudaccesstrailfull_notm}} 事件的動作" caption-side="top"}

## 在哪裡檢視事件
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} 事件提供於 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**，這提供於產生事件的 {{site.data.keyword.cloud_notm}} 地區。

例如，當您在 {{site.data.keyword.keymanagementserviceshort}} 中建立、匯入、刪除或讀取金鑰時，會產生 {{site.data.keyword.cloudaccesstrailshort}} 事件。這些事件會自動轉遞至與 {{site.data.keyword.keymanagementserviceshort}} 服務佈建所在地區相同之地區的 {{site.data.keyword.cloudaccesstrailshort}} 服務。

若要監視 API 活動，您必須在 {{site.data.keyword.keymanagementserviceshort}} 服務佈建所在之相同地區裡，可用的空間中佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務。然後，您便可以透過 {{site.data.keyword.cloudaccesstrailshort}} 使用者介面的帳戶視圖檢視事件（如果您具有精簡方案的話），以及透過 Kibana 來檢視事件（如果您具有超值方案的話）。

## 其他資訊
{: #activity-tracker-info}

由於加密金鑰的資訊機密性，當因為對 {{site.data.keyword.keymanagementserviceshort}} 服務的 API 呼叫而產生事件時，所產生的事件不會包含金鑰的相關詳細資訊。事件包含了一個相關性 ID，您可以用來在雲端環境中，在內部識別該金鑰。相關性 ID 是一個欄位，它是作為 `responseHeader.content` 欄位的一部分傳回。您可以使用此資訊讓 {{site.data.keyword.keymanagementserviceshort}} 金鑰與透過 {{site.data.keyword.cloudaccesstrailshort}} 事件報告之動作的資訊產生關聯。
