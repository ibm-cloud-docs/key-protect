---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# 匯入根金鑰
{: #import-root-keys}

您可以利用 {{site.data.keyword.keymanagementservicefull}}，以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 或使用 {{site.data.keyword.keymanagementserviceshort}} API 透過程式設計方式，來保護現有根金鑰。
{: shortdesc}

根金鑰是用來保護雲端中已加密資料安全的對稱金鑰包裝金鑰。如需將根金鑰匯入至 {{site.data.keyword.keymanagementserviceshort}} 的相關資訊，請參閱[自帶加密金鑰到雲端](/docs/services/key-protect?topic=key-protect-importing-keys)。

## 使用 GUI 匯入根金鑰
{: #import-root-key-gui}

[在建立服務的實例之後](/docs/services/key-protect?topic=key-protect-provision)，請完成下列步驟以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 來新增現有根金鑰。

1. [登入 {{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/){: new_window}。
2. 移至**功能表** &gt; **資源清單**以檢視資源的清單。
3. 從 {{site.data.keyword.cloud_notm}} 資源清單，選取已佈建的 {{site.data.keyword.keymanagementserviceshort}} 實例。
4. 若要匯入金鑰，請按一下**新增金鑰**，然後選取**匯入自己的金鑰**視窗。

    指定金鑰的詳細資料：

    <table>
      <tr>
        <th>設定</th>
        <th>說明</th>
      </tr>
      <tr>
        <td>名稱</td>
        <td>
          <p>方便識別金鑰且人類可閱讀的唯一別名。</p>
          <p>若要保護您的隱私權，請確定金鑰名稱未包含個人識別資訊 (PII)（例如您的姓名或位置）。</p>
        </td>
      </tr>
      <tr>
        <td>金鑰類型</td>
        <td>您要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">金鑰類型</a>。從金鑰類型清單中，選取<b>根金鑰</b>。</td>
      </tr>
      <tr>
        <td>金鑰資料</td>
        <td>
          <p>您要在服務中儲存及管理並以 base64 編碼的金鑰資料（例如現有金鑰包裝金鑰）。</p>
          <p>請確定金鑰資料滿足下列需求：</p>
          <p>
            <ul>
              <li>金鑰必須是 128、192 或 256 位元。</li>
              <li>必須使用 base64 編碼來編碼資料位元組（例如 32 位元組適用於 256 位元）。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 說明<b>匯入自己的金鑰</b>設定</caption>
    </table>

5. 當您填寫完金鑰的詳細資料時，請按一下**匯入金鑰**以便確認。 

## 使用 API 匯入根金鑰
{: #import-root-key-api}

您可以使用 {{site.data.keyword.keymanagementserviceshort}} API，將您的根金鑰匯入至服務。

藉由[檢閱建立及加密金鑰資料的選項](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)，來提前規劃匯入金鑰。若要新增安全，您可以在自帶金鑰資料到雲端之前，使用[傳輸金鑰](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys)來加密金鑰資料，以安全匯入金鑰資料。如果您偏好不使用傳輸金鑰來匯入根金鑰，請跳到[步驟 4](#import-root-key)。
{: note}

### 步驟 1：建立傳輸金鑰
{: #create-transport-key}

傳輸金鑰目前是測試版特性。測試版特性隨時可能會變更，未來更新項目可能會帶來一些變更，而這些變更與最新版本並不相容。
{: important}

對下列端點發出 `POST` 呼叫來建立服務實例的傳輸金鑰。

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
      <caption style="caption-side:bottom;">表 2. 說明建立傳輸金鑰 {{site.data.keyword.keymanagementserviceshort}} API 所需的變數</caption>
  </table>

  成功的 `POST api/v2/locker` 回應會傳回傳輸金鑰的 ID 值，以及其他 meta 資料。ID 是與傳輸金鑰相關聯的唯一 ID，用於後續呼叫 {{site.data.keyword.keymanagementserviceshort}} API。

### 步驟 2：擷取傳輸金鑰和匯入記號
{: #retrieve-transport-key}

對下列端點發出 `GET` 呼叫來擷取傳輸金鑰和匯入記號。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. 使用下列 cURL 指令，來呼叫 [{{site.data.keyword.keymanagementserviceshort}} API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>locker_ID</varname></td>
        <td><strong>必要。</strong>您在<a href="#create-transport-key">步驟 1</a> 中建立的傳輸金鑰的 ID。</td>
      </tr>
        <caption style="caption-side:bottom;">表 3. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 擷取傳輸金鑰所需的變數</caption>
    </table>

    成功的 `GET api/v2/lockers/{id}` 回應會以 PKIX 格式傳回 4096 位元、DER 編碼的公開加密金鑰，可用來加密您的根金鑰資料與用來驗證傳輸金鑰完整性的匯入記號。

### 步驟 3：加密金鑰資料
{: #encrypt-root-key}

擷取傳輸金鑰之後，請使用金鑰來加密您要匯入至 {{site.data.keyword.keymanagementserviceshort}} 的金鑰資料。  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

若要在內部部署時產生金鑰資料，[請檢閱用來建立對稱加密金鑰的選項](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)。例如，您可能想要使用由內部部署硬體安全模組 (HSM) 所支援的組織內部金鑰管理系統，來建立及匯出金鑰資料。
{: note}

若要加密金鑰資料，請執行下列動作：

1. 從內部部署金鑰管理系統，以二進位格式匯出 256 位元金鑰資料。

    若要瞭解如何建立及匯出金鑰資料，請參閱內部部署 HSM 或金鑰管理系統的文件。

2. 使用步驟 2 的[已擷取傳輸金鑰](#retrieve-transport-key)來加密金鑰資料。

   當您加密金鑰資料時，請使用 `RSAES_OAE_SHA_256` 加密方法。這是 {{site.data.keyword.keymanagementserviceshort}} 用來建立傳輸金鑰的預設方法。為了避免 {{site.data.keyword.keymanagementserviceshort}} 中的解密問題，當您對金鑰資料執行 RSIES_OAEP 加密時，請不要包含選用的 `label` 參數。若要瞭解如何在金鑰資料上執行 RSA 加密，請參閱內部部署 HSM 或金鑰管理系統的文件。

3. 繼續下一步之前，請確定已加密金鑰資料是 base64 編碼。

### 步驟 4：匯入金鑰資料
{: #import-root-key}

[對金鑰資料進行加密及 base64 編碼之後](#encrypt-root-key)，即可對下列端點發出 `POST` 呼叫，以將根金鑰匯入至 {{site.data.keyword.keymanagementserviceshort}}。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. 使用下列 cURL 指令，來呼叫 [{{site.data.keyword.keymanagementserviceshort}} API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
       }
     ]
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
        <td><varname>correlation_ID</varname></td>
        <td>用來追蹤及關聯交易的唯一 ID。</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>必要。</strong>方便識別金鑰且人類可閱讀的唯一名稱。為了保護您的隱私權，請不要將個人資料儲存為金鑰的 meta 資料。</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>金鑰的延伸說明。為了保護您的隱私權，請不要將個人資料儲存為金鑰的 meta 資料。</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>系統中的金鑰到期的日期和時間，以 RFC 3339 格式表示。如果省略 <code>expirationDate</code> 屬性，則金鑰不會到期。</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>您要在服務中儲存及管理並以 base64 編碼的金鑰資料（例如現有金鑰包裝金鑰）。</p>
          <p>請確定金鑰資料滿足下列需求：</p>
          <p>
            <ul>
              <li>金鑰必須是 128、192 或 256 位元。</li>
              <li>必須使用 base64 編碼來編碼資料位元組（例如 32 位元組適用於 256 位元）。</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>決定金鑰資料是否可以離開服務的布林值。</p>
          <p>當您將 <code>extractable</code> 屬性設為 <code>false</code> 時，服務會將金鑰指定為根金鑰，您可以將它用於 <code>wrap</code> 或 <code>unwrap</code> 作業。</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>您用來<a href="#encrypt-root-key">加密金鑰資料</a>的加密方法。目前，支援 <code>RSAES_OAAEP_SHA_256</code>。若要在不使用傳輸金鑰和匯入記號的情況下匯入根金鑰資料，請省略 <code>encryption_algorithm</code> 屬性。</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>匯入記號用來驗證傳輸金鑰是否活躍及其完整性。如果您使用傳輸金鑰來加密金鑰資料，則必須提供在<a href="#retrieve-transport-key">步驟 2</a> 中擷取的相同匯入記號。若要在不使用傳輸金鑰和匯入記號的情況下匯入根金鑰資料，請省略 <code>importToken</code> 屬性。</td>
      </tr>
        <caption style="caption-side:bottom;">表 4. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 新增根金鑰所需的變數</caption>
    </table>

    若要保護您個人資料的機密性，請在將金鑰新增至服務時避免輸入個人識別資訊 (PII)（例如您的姓名或位置）。如需其他 PII 範例，請參閱 [NIST 特殊出版品 800-122 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window} 的第 2.2 節。
    {: important}

    成功的 `POST api/v2/keys` 回應會傳回您金鑰的 ID 值，以及其他 meta 資料。ID 是指派給您金鑰的唯一 ID，並用於後續的 {{site.data.keyword.keymanagementserviceshort}} API 呼叫。

2. 選用項目：執行下列呼叫來瀏覽 {{site.data.keyword.keymanagementserviceshort}} 服務實例中的金鑰，確認已新增金鑰。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 下一步為何？
{: #import-root-key-next-steps}

- 若要進一步瞭解如何使用封套加密來保護金鑰，請參閱[包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys)。
- 若要進一步瞭解如何以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。
