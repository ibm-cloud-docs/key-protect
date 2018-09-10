---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at-events}

請使用 {{site.data.keyword.cloudaccesstrailfull}} 服務來追蹤使用者及應用程式與 {{site.data.keyword.keymanagementservicefull}} 的互動情況。
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} 服務會記錄由使用者起始，且會變更 {{site.data.keyword.cloud_notm}} 中服務狀態的活動。 

如需相關資訊，請參閱 [{{site.data.keyword.cloudaccesstrailshort}} 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla){: new_window}。

## 事件清單
{: #events}

下表列出會產生事件的動作：

<table>
    <tr>
        <th>動作</th>
        <th>說明</th>
    </tr>
    <tr>
        <td>kms.secrets.create</td>
        <td>建立金鑰</td>
    </tr>
    <tr>
        <td>kms.secrets.read</td>
        <td>依 ID 擷取金鑰</td>
    </tr>
   <tr>
        <td>kms.secrets.delete</td>
        <td>依 ID 刪除金鑰</td>
    </tr>
    <tr>
        <td>kms.secrets.list</td>
        <td>擷取金鑰清單</td>
    </tr>
    <tr>
        <td>kms.secrets.head</td>
        <td>擷取金鑰數目</td>
    </tr>
     <tr>
        <td>kms.secrets.wrap</td>
        <td>包裝金鑰</td>
    </tr>
     <tr>
        <td>kms.secrets.unwrap</td>
        <td>解除包裝金鑰</td>
    </tr>
    <caption style="caption-side:bottom;">表 1. 產生 {{site.data.keyword.cloudaccesstrailfull_notm}} 事件的動作</caption>
</table>

## 在哪裡檢視事件
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} 事件提供於 {{site.data.keyword.cloudaccesstrailshort}} **帳戶網域**，這提供於產生事件的 {{site.data.keyword.cloud_notm}} 地區。

例如，當您在 {{site.data.keyword.keymanagementserviceshort}} 中建立、匯入、刪除或讀取金鑰時，會產生 {{site.data.keyword.cloudaccesstrailshort}} 事件。這些事件會自動轉遞至與 {{site.data.keyword.keymanagementserviceshort}} 服務佈建所在地區相同之地區的 {{site.data.keyword.cloudaccesstrailshort}} 服務。

若要監視 API 活動，您必須在 {{site.data.keyword.keymanagementserviceshort}} 服務佈建所在之相同地區裡，可用的空間中佈建 {{site.data.keyword.cloudaccesstrailshort}} 服務。然後，您便可以透過 {{site.data.keyword.cloudaccesstrailshort}} 使用者介面的帳戶視圖檢視事件（如果您具有精簡方案的話），以及透過 Kibana 來檢視事件（如果您具有超值方案的話）。

## 其他資訊
{: #info}

由於加密金鑰的資訊機密性，當因為對 {{site.data.keyword.keymanagementserviceshort}} 服務的 API 呼叫而產生事件時，所產生的事件不會包含金鑰的相關詳細資訊。事件包含了一個相關性 ID，您可以用來在雲端環境中，在內部識別該金鑰。相關性 ID 是一個欄位，它是作為 `responseHeader.content` 欄位的一部分傳回。您可以使用此資訊讓 {{site.data.keyword.keymanagementserviceshort}} 金鑰與透過 {{site.data.keyword.cloudaccesstrailshort}} 事件報告之動作的資訊產生關聯。
