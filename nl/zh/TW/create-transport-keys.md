---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# 建立傳輸金鑰
{: #create-transport-keys}

您可以先為 {{site.data.keyword.keymanagementserviceshort}} 服務實例建立一個傳輸加密金鑰，以讓根金鑰資料安全匯入雲端中。
{: shortdesc}

傳輸金鑰用於加密，並根據您指定的原則，將根金鑰資料安全地匯入至 {{site.data.keyword.keymanagementserviceshort}}。若要進一步瞭解如何將金鑰安全地匯入至雲端中，請參閱[自帶加密金鑰到雲端](/docs/services/key-protect/concepts?topic=key-protect-importing-keys)。

傳輸金鑰目前是測試版特性。測試版特性隨時可能會變更，未來更新項目可能會帶來一些變更，而這些變更與最新版本並不相容。
{: important}

## 使用 API 建立傳輸金鑰
{: #create-transport-key-api}

對下列端點發出 `POST` 呼叫來建立與 {{site.data.keyword.keymanagementserviceshort}} 服務實例相關聯的傳輸金鑰。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [擷取服務及鑑別認證以在服務中使用金鑰](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 藉由呼叫 [{{site.data.keyword.keymanagementserviceshort}} API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window} 來為傳輸金鑰設定原則。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

    根據下表取代範例要求中的變數。

      <table>
        <tr>
          <th>變數</th>
          <th>說明</th>
        </tr>
        <tr>
          <td><varname>region</varname></td>
          <td><strong>必要。</strong>代表 {{site.data.keyword.keymanagementserviceshort}} 服務實例所在地理區域的地區縮寫，例如 <code>us-south</code> 或 <code>eu-gb</code>。如需相關資訊，請參閱<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地區服務端點</a>。</td>
        </tr>
        <tr>
          <td><varname>IAM_token</varname></td>
          <td><strong>必要。</strong>您的 {{site.data.keyword.cloud_notm}} 存取記號。請在 cURL 要求中包含 <code>IAM</code> 記號的完整內容，包括 Bearer 值。如需相關資訊，請參閱<a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">擷取存取記號</a>。</td>
        </tr>
        <tr>
          <td><varname>instance_ID</varname></td>
          <td><strong>必要。</strong>指派給您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例的唯一 ID。如需相關資訊，請參閱<a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">擷取實例 ID</a>。</td>
        </tr>
        <tr>
          <td><varname>expiration_time</varname></td>
          <td>
            <p>建立傳輸金鑰開始算起的時間（以秒為單位），用於判定金鑰保持有效的時間。</p>
            <p>最小值是 300 秒（5 分鐘），最大值是 86400 秒（24 小時）。預設值是 600 秒（10 分鐘）。</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>在傳輸金鑰無法再存取之前，可在其有效期限內擷取傳輸金鑰的次數。預設值是 1。</td>
        </tr>
          <caption style="caption-side:bottom;">表 1. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 新增根金鑰所需的變數</caption>
      </table>

    成功的 `POST api/v2/locker` 要求會為服務實例建立一個傳輸金鑰，並傳回其 ID 值與其他 meta 資料。ID 是與傳輸金鑰相關聯的唯一 ID，用於後續呼叫 {{site.data.keyword.keymanagementserviceshort}} API。

3. 選用項目：驗證已透過執行下列呼叫來建立傳輸金鑰，以擷取服務實例的相關 meta 資料。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 下一步為何？
{: #create-transport-key-next-steps}

- 若要進一步瞭解如何使用傳輸金鑰將根金鑰匯入至服務中，請參閱[匯入根金鑰](/docs/services/key-protect?topic=key-protect-import-root-keys)。
- 若要進一步瞭解如何以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。
