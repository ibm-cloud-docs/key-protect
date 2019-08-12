---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# 擷取實例 ID
{: #retrieve-instance-ID}

您可以在對服務的 API 要求中，包括其唯一 ID 或實例 ID，以將個別 {{site.data.keyword.keymanagementservicelong}} 服務實例設為目標以進行作業。
{: shortdesc}

## 在 {{site.data.keyword.cloud_notm}} 主控台中檢視實例 ID
{: #view-instance-ID}

您可以導覽至 {{site.data.keyword.cloud_notm}} 資源清單，以檢視與 {{site.data.keyword.keymanagementserviceshort}} 服務實例相關聯的實例 ID。

1. [登入 {{site.data.keyword.cloud_notm}} 主控台](https://{DomainName}){: external}。
2. 移至**功能表** &gt; **資源清單**，然後按一下**服務**，以瀏覽您的雲端服務清單。
3. 按一下說明您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例的表格列。
4. 從服務詳細資料視圖中，複製 **GUID** 值。

    這個 **GUID** 值代表用來唯一識別 {{site.data.keyword.keymanagementserviceshort}} 服務實例的實例 ID。

## 使用 CLI 擷取實例 ID
{: #retrieve-instance-ID-cli}

您也可以使用 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external} 來擷取服務實例的實例 ID。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external} 來登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

2. 選取包含 {{site.data.keyword.keymanagementserviceshort}} 之佈建實例的帳戶、地區和資源群組。

3. 擷取可唯一識別 {{site.data.keyword.keymanagementserviceshort}} 服務實例的「雲端資源名稱 (CRN)」。 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    將 `<instance_name>` 取代為您指派給 {{site.data.keyword.keymanagementserviceshort}} 服務實例的唯一別名。下列縮減的範例顯示 CLI 輸出。

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    _42454b3b-5b06-407b-a4b3-34d9ef323901_ 值是範例實例 ID。


## 使用 API 擷取實例 ID
{: #retrieve-instance-ID-api}

您可能想要以程式設計方式擷取實例 ID，以協助您建置及連接應用程式。您可以呼叫 [{{site.data.keyword.cloud_notm}} Resource Controller API](https://{DomainName}/apidocs/resource-controller)，然後將 JSON 輸出傳送至 `jq`，以擷取此值。

1. [擷取 {{site.data.keyword.cloud_notm}} IAM 存取記號](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. 呼叫 [Resource Controller API](https://{DomainName}/apidocs/resource-controller) 以擷取您的實例 ID。

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    將 `<instance_name>` 取代為您指派給 {{site.data.keyword.keymanagementserviceshort}} 服務實例的唯一別名。下列輸出顯示範例實例 ID：

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
