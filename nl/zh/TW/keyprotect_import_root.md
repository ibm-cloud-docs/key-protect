---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 匯入主要金鑰
{: #import_root_keys}

您可以利用 {{site.data.keyword.keymanagementservicefull}}，以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 或使用 {{site.data.keyword.keymanagementserviceshort}} API 透過程式設計方式，來保護現有主要金鑰。
{: shortdesc}

主要金鑰是用來保護雲端中已加密資料安全的對稱金鑰包裝金鑰。如需主要金鑰的相關資訊，請參閱[封套加密](/docs/services/keymgmt/concepts/keyprotect_envelope.html)。 

## 使用 GUI 匯入主要金鑰
{: #import_root_key_GUI}

[在建立服務的實例之後](/docs/services/keymgmt/keyprotect_provision.html)，請完成下列步驟以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 來新增現有主要金鑰。

1. [登入 {{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/){: new_window}。
2. 從 {{site.data.keyword.cloud_notm}} 儀表板，選取已佈建的 {{site.data.keyword.keymanagementserviceshort}} 實例。
3. 若要匯入金鑰，請按一下**新增金鑰**，然後選取**輸入現有金鑰**視窗。

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
        <td>您要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types">金鑰類型</a>。從金鑰類型清單中，選取<b>主要金鑰</b>。</td>
      </tr>
      <tr>
        <td>金鑰資料</td>
        <td>
          <p>您要在服務中儲存及管理並以 base64 編碼的金鑰資料（例如現有金鑰包裝金鑰）。</p>
          <p>請確定金鑰資料滿足下列需求：</p>
          <p>
            <ul>
              <li>金鑰必須是 256、384 或 512 位元。</li>
              <li>必須使用 base64 編碼來編碼資料位元組（例如 32 位元組適用於 256 位元）。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 說明<b>輸入現有金鑰</b>設定</caption>
    </table>

4. 當您填寫完金鑰的詳細資料時，請按一下**新增金鑰**以便確認。 

## 使用 API 匯入主要金鑰
{: #import_root_key_API}

對下列端點發出 `POST` 呼叫來新增現有主要金鑰。

```
https://keyprotect.<region>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [擷取服務及鑑別認證以在服務中使用金鑰](/docs/services/keymgmt/keyprotect_authentication.html)。

2. 使用下列 cURL 指令，來呼叫 [{{site.data.keyword.keymanagementserviceshort}} API ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/apidocs/639){: new_window}。

    ```cURL
    curl -X POST \
      https://keyprotect.<region>.bluemix.net/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
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
       "extractable": <key_type>
       }
     ]
    }'
    ```
    {: codeblock}

    若要在您帳戶的 Cloud Foundry 組織及空間內使用金鑰，請將 `Bluemix-Instance` 取代為適當的 `Bluemix-org` 及 `Bluemix-space` 標頭。[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件以取得程式碼範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/apidocs/639){: new_window}。
    {: tip}

    根據下表取代範例要求中的變數。
    <table>
      <tr>
        <th>變數</th>
        <th>說明</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td>代表 {{site.data.keyword.keymanagementserviceshort}} 服務實例所在地理區域的地區縮寫，例如 <code>us-south</code> 或 <code>eu-gb</code>。如需相關資訊，請參閱<a href="/docs/services/keymgmt/keyprotect_regions.html#endpoints">地區服務端點</a>。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td>您的 {{site.data.keyword.cloud_notm}} 存取記號。請在 cURL 要求中包含 <code>IAM</code> 記號的完整內容，包括 Bearer 值。如需相關資訊，請參閱<a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token">擷取存取記號</a>。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td>指派給您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例的唯一 ID。如需相關資訊，請參閱<a href="/docs/services/keymgmt/keyprotect_authentication.html#retrieve_instance_ID">擷取實例 ID</a>。</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>用來追蹤及關聯交易的唯一 ID。</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td>
          <p>方便識別金鑰且人類可閱讀的唯一名稱。</p>
          <p>重要事項：若要保護您的隱私權，請不要將個人資料儲存為金鑰的 meta 資料。</p>
        </td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>
          <p>選用項目：金鑰的延伸說明。</p>
          <p>重要事項：若要保護您的隱私權，請不要將個人資料儲存為金鑰的 meta 資料。</p>
        </td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>選用項目：系統中的金鑰到期的日期和時間，以 RFC 3339 格式表示。如果省略 <code>expirationDate</code> 屬性，則金鑰不會到期。</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>您要在服務中儲存及管理並以 base64 編碼的金鑰資料（例如現有金鑰包裝金鑰）。</p>
          <p>請確定金鑰資料滿足下列需求：</p>
          <p>
            <ul>
              <li>金鑰必須是 256、384 或 512 位元。</li>
              <li>必須使用 base64 編碼來編碼資料位元組（例如 32 位元組適用於 256 位元）。</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>決定金鑰資料是否可以離開服務的布林值。</p>
          <p>當您將 <code>extractable</code> 屬性設為 <code>false</code> 時，服務會將金鑰指定為主要金鑰，您可以將它用於 <code>wrap</code> 或 <code>unwrap</code> 作業。</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 1. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 新增主要金鑰所需的變數</caption>
    </table>

    若要保護您個人資料的機密性，請在將金鑰新增至服務時避免輸入個人識別資訊 (PII)（例如您的姓名或位置）。如需其他 PII 範例，請參閱 [NIST 特殊出版品 800-122 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window} 的第 2.2 節。
    {: tip}

    成功的 `POST /v2/keys` 回應會傳回您金鑰的 ID 值，以及其他 meta 資料。ID 是指派給您金鑰的唯一 ID，並用於後續的 {{site.data.keyword.keymanagementserviceshort}} API 呼叫。

3. 選用項目：執行下列呼叫來瀏覽 {{site.data.keyword.keymanagementserviceshort}} 服務實例中的金鑰，確認已新增金鑰。

    ```cURL
    curl -X GET \
      https://keyprotect.<region>.bluemix.net/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

**附註：**當您將現有主要金鑰新增至服務時，金鑰會保留在 {{site.data.keyword.keymanagementserviceshort}} 的範圍內，而且無法擷取其金鑰資料。 

### 下一步為何？

- 若要進一步瞭解如何使用封套加密來保護金鑰，請參閱[包裝金鑰](/docs/services/keymgmt/keyprotect_wrap_keys.html)。
- 若要進一步瞭解以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件以取得程式碼範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/apidocs/639){: new_window}。
