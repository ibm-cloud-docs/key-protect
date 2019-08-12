---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# 設定 API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} 提供的 REST API 可與任何程式設計語言搭配使用，以儲存、擷取及產生加密金鑰。
{: shortdesc}

## 擷取您的 {{site.data.keyword.cloud_notm}} 認證
{: #retrieve-credentials}

若要使用 API，您需要產生服務及鑑別認證。 

若要收集您的認證，請執行下列動作：

1. [產生一個 {{site.data.keyword.cloud_notm}} IAM 存取記號](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. [擷取用來唯一識別 {{site.data.keyword.keymanagementserviceshort}} 服務實例](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID)的實例 ID。

## 形成 API 要求
{: #form-api-request}

對服務發出 API 呼叫時，會根據最初佈建 {{site.data.keyword.keymanagementserviceshort}} 實例的方式來建構 API 要求。 

若要建置您的要求，請將[地區服務端點](/docs/services/key-protect?topic=key-protect-regions)與適當的鑑別認證配對。例如，如果您已建立 `us-south` 地區的服務實例，請使用下列端點及 API 標頭來瀏覽服務中的金鑰：

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

以擷取的服務與鑑別認證來取代 `<access_token>` 與 `<instance_ID>`。

想要在發生錯誤時追蹤您的 API 要求嗎？當您在 cURL 要求中包括 `-v` 旗標時，即會在回應標頭中獲得 `correlation-id` 值。您可以使用此值來產生關聯，並追蹤要求以進行除錯。
{: tip} 

## 下一步為何？
{: #set-up-api-next-steps}

您已準備好開始在 Key Protect 中管理加密金鑰。若要進一步瞭解 [ 中透過程式設計方式來管理金鑰的相關資訊，請參閱{{site.data.keyword.keymanagementserviceshort}} API 參考文件](https://{DomainName}/apidocs/key-protect){: external}。
