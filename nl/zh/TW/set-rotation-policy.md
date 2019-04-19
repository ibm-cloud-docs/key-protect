---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# 設定替換原則
{: #set-rotation-policy}

您可以使用 {{site.data.keyword.keymanagementservicefull}} 為根金鑰設定自動替換原則。
{: shortdesc}

當您為根金鑰設定自動替換原則時，會定期縮短金鑰的生命期限，並且會限制該金鑰所保護的資訊量。

您只能為 {{site.data.keyword.keymanagementserviceshort}} 中產生的根金鑰建立替換原則。如果您一開始匯入根金鑰，則必須提供新的 base64 編碼金鑰資料才能替換金鑰。如需相關資訊，請參閱[隨需應變替換根金鑰](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys)。
{: note}

想要進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} 中的金鑰替換選項嗎？請參閱[比較金鑰替換選項](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)，以取得更多資訊。
{: tip}

## 在 GUI 中管理替換原則
{: #manage-policies-gui}

如果您偏好使用圖形介面來管理根金鑰的原則，可以使用 {{site.data.keyword.keymanagementserviceshort}} GUI。

1. [登入 {{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/){: new_window}。
2. 移至**功能表** &gt; **資源清單**以檢視資源的清單。
3. 從 {{site.data.keyword.cloud_notm}} 資源清單，選取已佈建的 {{site.data.keyword.keymanagementserviceshort}} 實例。
4. 在應用程式詳細資料頁面上，使用**金鑰**表格，以瀏覽服務中的金鑰。
5. 按一下 ⋯ 圖示，以開啟特定金鑰的選項清單。
6. 從選項功能表中，按一下**管理原則**來管理金鑰的替換原則。
7. 從替換選項清單中，選取替換頻率（以月為單位）。

    如果您的金鑰具有現有替換原則，則介面會顯示金鑰的現有替換期間。

8. 按一下**建立原則**，來為金鑰設定原則。

當要根據您指定的替換間隔來替換金鑰時，{{site.data.keyword.keymanagementserviceshort}} 會自動將根金鑰取代為新的金鑰資料。

## 使用 API 管理替換原則
{: #manage-rotation-policies-api}

### 檢視替換原則
{: #view-rotation-policy-api}

若為高階視圖，您可以對下列端點發出 `GET` 呼叫，以瀏覽與根金鑰相關聯的原則。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [擷取服務和鑑別認證](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 執行下列 cURL 指令，以擷取指定金鑰的替換原則。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    根據下表取代範例要求中的變數。
    <table>
      <tr>
        <th>變數</th>
        <th>說明</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>具有現有替換原則的根金鑰的唯一 ID。</td>
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
        <caption style="caption-side:bottom;">表 1. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 建立替換原則所需的變數</caption>
    </table>

    成功的 `GET api/v2/keys/{id}/policies` 回應會傳回與您的金鑰相關聯的原則詳細資料。下列 JSON 物件顯示具有現有替換原則的根金鑰的範例回應。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
            {
                "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    `interval_month` 值指出金鑰替換頻率（以月為單位）。

### 建立替換原則
{: #create-rotation-policy-api}

對下列端點發出 `PUT` 呼叫來建立根金鑰的替換原則。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [擷取服務和鑑別認證](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 執行下列 cURL 指令，為指定的金鑰建立替換原則。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>您要為其建立替換原則的根金鑰的唯一 ID。</td>
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
        <td><varname>rotation_interval</varname></td>
        <td><strong>必要。</strong>這是一個整數值，用於判定金鑰替換間隔時間（以月為單位）。最小值是 <code>1</code>，最大值是 <code>12</code>。</td>
      </tr>
        <caption style="caption-side:bottom;">表 1. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 建立替換原則所需的變數</caption>
    </table>

    成功的 `PUT api/v2/keys/{id}/policies` 回應會傳回與您的金鑰相關聯的原則詳細資料。下列 JSON 物件顯示具有現有替換原則的根金鑰的範例回應。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
            {
                "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### 更新替換原則
{: #update-rotation-policy-api}

對下列端點發出 `PUT` 呼叫來更新根金鑰的現有原則。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [擷取服務和鑑別認證](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 執行下列 cURL 指令，以取代指定金鑰的替換原則。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必要。</strong>您要取代其替換原則的根金鑰的唯一 ID。</td>
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
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>必要。</strong>這是一個整數值，用於判定金鑰替換間隔時間（以月為單位）。最小值是 <code>1</code>，最大值是 <code>12</code>。</td>
      </tr>
        <caption style="caption-side:bottom;">表 1. 說明使用 {{site.data.keyword.keymanagementserviceshort}} API 建立替換原則所需的變數</caption>
    </table>

    成功的 `PUT api/v2/keys/{id}/policies` 回應會傳回與您的金鑰相關聯的更新原則詳細資料。下列 JSON 物件顯示具有已更新替換原則的根金鑰的範例回應。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
            {
                "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    在金鑰的原則詳細資料中會更新 `interval_month` 和 `updatedat` 值。如果其他使用者更新您最初建立之金鑰的原則，則 `updatedby` 值也會變更，以顯示傳送要求的人員 ID。
