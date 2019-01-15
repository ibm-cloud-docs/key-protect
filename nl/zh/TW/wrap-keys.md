---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 包裝金鑰
{: #wrap-keys}

如果您是特許使用者，則可以使用 {{site.data.keyword.keymanagementservicelong}} API，以利用主要金鑰來管理及保護加密金鑰。
{: shortdesc}

當您使用主要金鑰來包裝資料加密金鑰 (DEK) 時，{{site.data.keyword.keymanagementserviceshort}} 會結合多個演算法的長處，來保護已加密資料的隱私權及完整性。  

若要瞭解金鑰包裝如何協助您控制雲端中靜置資料的安全，請參閱[封套加密](/docs/services/key-protect/concepts/envelope-encryption.html)。

## 使用 API 包裝金鑰
{: #api}

您可以使用您在 {{site.data.keyword.keymanagementserviceshort}} 中管理的主要金鑰來保護指定的資料加密金鑰 (DEK)。

當您提供用於包裝的主要金鑰時，請確定主要金鑰是 256、384 或 512 位元，以讓 wrap 呼叫成功。如果您在服務中建立主要金鑰，則 {{site.data.keyword.keymanagementserviceshort}} 會從 AES-GCM 演算法所支援的 HSM 產生 256 位元金鑰。
{: note}

[在服務中指定主要金鑰之後](/docs/services/key-protect/create-root-keys.html)，即可對下列端點發出 `POST` 呼叫，以使用進階加密來包裝 DEK。

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [擷取服務及鑑別認證以在服務中使用金鑰](/docs/services/key-protect/access-api.html)。

2. 複製您要管理及保護之 DEK 的金鑰資料。

    如果您具有 {{site.data.keyword.keymanagementserviceshort}} 服務實例的管理員或作者專用權，則[可以提出 `GET /v2/keys/<key_ID>` 要求來擷取特定金鑰的金鑰資料](/docs/services/key-protect/view-keys.html#api)。

3. 複製您要用於包裝之主要金鑰的 ID。

4. 執行下列 cURL 指令，以使用 wrap 作業來保護金鑰。

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    若要在您帳戶的 Cloud Foundry 組織及空間內使用金鑰，請將 `Bluemix-Instance` 取代為適當的 `Bluemix-org` 及 `Bluemix-space` 標頭。[如需相關資訊，請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。
    {: tip}

    根據下表取代範例要求中的變數。

    <table>
      <tr>
        <th>變數</th>
        <th>說明</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>必要。</strong>代表 {{site.data.keyword.keymanagementserviceshort}} 服務實例所在地理區域的地區縮寫，例如 <code>us-south</code> 或 <code>eu-gb</code>。如需相關資訊，請參閱<a href="/docs/services/key-protect/regions.html#endpoints">地區服務端點</a>。</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>您要用於包裝之主要金鑰的唯一 ID。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>必要。</strong>您的 {{site.data.keyword.cloud_notm}} 存取記號。請在 cURL 要求中包含 <code>IAM</code> 記號的完整內容，包括 Bearer 值。如需相關資訊，請參閱<a href="/docs/services/key-protect/access-api.html#retrieve-token">擷取存取記號</a>。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>必要。</strong>指派給您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例的唯一 ID。如需相關資訊，請參閱<a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">擷取實例 ID</a>。</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>用來追蹤及關聯交易的唯一 ID。</td>
      </tr>
      <tr>
        <td><varname>data_key</varname></td>
        <td>您要管理及保護之 DEK 的金鑰資料。<code>plaintext</code> 值必須以 base64 編碼。若要產生新的 DEK，請省略 <code>plaintext</code> 屬性。此服務會產生隨機純文字（32 個位元組）、包裝該值，然後在回應中同時傳回所產生和包裝的值。</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>用來進一步保護金鑰的其他鑑別資料 (AAD)。每一個字串最多可以保留 255 個字元。如果您在對服務發出 wrap 呼叫時提供 AAD，則必須在後續 unwrap 呼叫期間指定相同的 AAD。<br></br>重要事項：{{site.data.keyword.keymanagementserviceshort}} 服務不會儲存其他的鑑別資料。如果您提供 AAD，請將資料儲存到安全的位置，以確保您可以在後續的 unwrap 要求期間存取及提供相同的 AAD。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 說明在 {{site.data.keyword.keymanagementserviceshort}} 中包裝所指定金鑰所需的變數。</caption>
    </table>

    已包裝資料加密金鑰（包含以 base64 編碼的金鑰資料）會在回應實體內文中傳回。下列 JSON 物件顯示回覆值範例。

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    如果您在提出 wrap 要求時省略 `plaintext` 屬性，則服務會以 base64 編碼格式傳回產生的資料加密金鑰 (DEK) 和已包裝 DEK。

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    <code>plaintext</code> 值代表未包裝的 DEK，<code>ciphertext</code> 值代表已包裝 DEK。
    
    如果您想要 {{site.data.keyword.keymanagementserviceshort}} 代表您產生新的資料加密金鑰 (DEK)，您也可以在 wrap 要求傳入空的內文。所產生的 DEK（包含以 base64 編碼的金鑰資料）會在回應實體內文中傳回，並且隨附已包裝的 DEK。
    {: tip}
    
