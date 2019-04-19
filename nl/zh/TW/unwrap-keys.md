---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# 解除包裝金鑰
{: #unwrap-keys}

如果您是特許使用者，則可以使用 {{site.data.keyword.keymanagementservicefull}} API，以解除包裝資料加密金鑰 (DEK) 來存取其內容。解除包裝 DEK 會解密其內容並檢查內容完整性，並將原始金鑰資料傳回給 {{site.data.keyword.cloud_notm}} 資料服務。
{: shortdesc}

若要瞭解金鑰包裝如何協助您控制雲端中靜置資料的安全，請參閱[使用封套加密保護資料](/docs/services/key-protect?topic=key-protect-envelope-encryption)。

## 使用 API 解除包裝金鑰
{: #unwrap-key-api}

[對服務發出 wrap 呼叫之後](/docs/services/key-protect?topic=key-protect-wrap-keys)，即可對下列端點發出 `POST` 呼叫，以解除包裝指定的資料加密金鑰 (DEK) 來存取其內容。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [擷取服務及鑑別認證以在服務中使用金鑰](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 複製您用來執行起始 wrap 要求之根金鑰的 ID。

    您可以擷取金鑰的 ID，方法是提出 `GET /v2/keys` 要求，或在 {{site.data.keyword.keymanagementserviceshort}} GUI 中檢視金鑰。

3. 複製在起始 wrap 要求期間所傳回的 `ciphertext` 值。

4. 執行下列 cURL 指令，以解密及鑑別金鑰資料。

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
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
        <td><strong>必要。</strong>代表 {{site.data.keyword.keymanagementserviceshort}} 服務實例所在地理區域的地區縮寫，例如 <code>us-south</code> 或 <code>eu-gb</code>。如需相關資訊，請參閱<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地區服務端點</a>。</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>您用於起始 wrap 要求之根金鑰的唯一 ID。</td>
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
        <td><varname>correlation_ID</varname></td>
        <td>用來追蹤及關聯交易的唯一 ID。</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>用來進一步保護金鑰的其他鑑別資料 (AAD)。每一個字串最多可以保留 255 個字元。如果您在對服務發出 wrap 呼叫時提供 AAD，則必須在 unwrap 呼叫期間指定相同的 AAD。</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>必要。</strong>在 wrap 作業期間所傳回的 <code>ciphertext</code> 值。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 說明在 {{site.data.keyword.keymanagementserviceshort}} 中解除包裝金鑰所需的變數。</caption>
    </table>

    原始金鑰資料會在回應實體內文中傳回。下列 JSON 物件顯示回覆值範例。

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}
