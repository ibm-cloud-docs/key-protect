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

# 替換金鑰
{: #rotating-keys}

您可以使用 {{site.data.keyword.keymanagementservicefull}} 隨需應變替換主要金鑰。
{: shortdesc}

當您替換主要金鑰時，會縮短金鑰的生命期限，並且會限制該金鑰所保護的資訊量。   

若要了解金鑰替換如何協助您符合業界標準和加密最佳作法，請參閱[金鑰替換](/docs/services/key-protect/concepts/key-rotation.html)。

替換只適用於主要金鑰。
{: note}

## 使用 GUI 替換主要金鑰
{: #gui}

如果您偏好使用圖形介面來替換主要金鑰，可以使用 {{site.data.keyword.keymanagementserviceshort}} GUI。

[在建立金鑰或將現有主要金鑰匯入到服務之後](/docs/services/key-protect/create-root-keys.html)，請完成下列步驟來替換金鑰：

1. [登入 {{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/){: new_window}。
2. 移至**功能表** &gt; **資源清單**以檢視資源的清單。
3. 從 {{site.data.keyword.cloud_notm}} 資源清單，選取已佈建的 {{site.data.keyword.keymanagementserviceshort}} 實例。
4. 在應用程式詳細資料頁面上，使用**金鑰**表格，以瀏覽服務中的金鑰。
5. 按一下 ⋮ 圖示，以開啟您要替換之金鑰的選項清單。
6. 從選項功能表，按一下**替換金鑰**，並在下一個畫面中確認替換。

如果您一開始匯入主要金鑰，則必須提供新的 base64 編碼金鑰資料才能替換金鑰。如需相關資訊，請參閱[使用 GUI 匯入主要金鑰](/docs/services/key-protect/import-root-keys.html#gui)。
{: note}

## 使用 API 替換主要金鑰
{: #api}

[在服務中指定主要金鑰之後](/docs/services/key-protect/create-root-keys.html)，即可對下列端點發出 `POST` 呼叫以替換金鑰。

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [擷取服務及鑑別認證以在服務中使用金鑰](/docs/services/key-protect/access-api.html)。

2. 複製您要替換之主要金鑰的 ID。

4. 執行下列 cURL 指令，以使用新的金鑰資料來取代金鑰。

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <key_material>'
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
        <td><strong>必要。</strong>代表 {{site.data.keyword.keymanagementserviceshort}} 服務實例所在地理區域的地區縮寫，例如 <code>us-south</code> 或 <code>eu-gb</code>。如需相關資訊，請參閱<a href="/docs/services/key-protect/regions.html#endpoints">地區服務端點</a>。</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>您要替換之主要金鑰的唯一 ID。</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>您要在服務中儲存及管理並以 base64 編碼的金鑰資料。如果您一開始將金鑰新增至服務時匯入了金鑰資料，則此值為必要。</p>
          <p>若要替換一開始由 {{site.data.keyword.keymanagementserviceshort}} 產生的金鑰，請省略 <code>payload</code> 屬性，並傳遞空的要求實體內文。若要替換已匯入的金鑰，請提供符合下列需求的金鑰資料：</p>
          <p>
            <ul>
              <li>金鑰必須是 256、384 或 512 位元。</li>
              <li>必須使用 base64 編碼來編碼資料位元組（例如 32 位元組適用於 256 位元）。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 說明在 {{site.data.keyword.keymanagementserviceshort}} 中替換所指定金鑰所需的變數。</caption>
    </table>

    成功的替換要求會傳回 HTTP `204 No Content` 回應，指出您的主要金鑰已取代為新的金鑰資料。

4. 選用項目：執行下列呼叫來瀏覽 {{site.data.keyword.keymanagementserviceshort}} 服務實例中的金鑰，確認已替換金鑰。

    ```cURL
    curl -X GET \
    https://keyprotect.<region>.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}
  
    請檢閱回應實體內文中的 `lastRotateDate` 值，以檢查您的金鑰前次替換的日期和時間。
    
