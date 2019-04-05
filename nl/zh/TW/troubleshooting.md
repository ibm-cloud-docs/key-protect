---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 疑難排解
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} 的一般使用問題可能包括在您與 API 互動時提供正確的標頭或認證。在許多情況下，您都可以遵循一些簡單的步驟來回復這些問題。
{: shortdesc}

## 無法刪除我的 Cloud Foundry 服務實例
{: #unable-to-delete-service}

當您嘗試刪除 {{site.data.keyword.keymanagementserviceshort}} 服務實例時，服務無法如預期地刪除。

從 {{site.data.keyword.cloud_notm}} 儀表板，導覽至 **Cloud Foundry 服務**，然後選取 {{site.data.keyword.keymanagementserviceshort}} 的實例。您按一下 ⋮ 圖示，即可開啟服務供應項目的選項清單，然後按一下**刪除服務**。
{: tsSymptoms}

服務無法刪除，您看到下列錯誤： 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

在 2017 年 12 月 15 日，{{site.data.keyword.keymanagementserviceshort}} 已經從使用 Cloud Foundry 組織、空間及角色，改為使用 IAM 及資源群組。您現在可以在資源群組裡佈建 {{site.data.keyword.keymanagementserviceshort}} 服務，而不需要指定 Cloud Foundry 組織及空間。
{: tsCauses}

這些變更已影響較舊服務實例的取消佈建運作方式。如果您已在 2017 年 9 月 28 日之前建立 {{site.data.keyword.keymanagementserviceshort}} 實例，則服務刪除可能不會如預期地運作。

若要刪除較舊的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，您必須先使用舊式 `https://ibm-key-protect.edge.bluemix.net` 端點刪除現有的金鑰，才能與 {{site.data.keyword.keymanagementserviceshort}} 服務互動。
{: tsResolve}

若要刪除金鑰及服務實例，請執行下列動作：

1. 使用 {{site.data.keyword.cloud_notm}} CLI 登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **附註：**如果登入失敗，請執行 `bx login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。

2. 選取包含 {{site.data.keyword.keymanagementserviceshort}} 服務實例的 {{site.data.keyword.cloud_notm}} 地區、組織及空間。

    記下 CLI 輸出中的組織及空間名稱。您也可以執行 `ibmcloud cf target` 來設定目標 Cloud Foundry 組織及空間。

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. 擷取 {{site.data.keyword.cloud_notm}} 組織及空間 GUID。

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
    將 `<organization_name>` 及 `<space_name>` 取代為指派給您組織及空間的唯一別名。

4. 擷取您的存取記號。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. 執行下列 cURL 指令，列出儲存在您服務實例中的金鑰。

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    將 `<access_token>`, `<organization_GUID>` 及 `<space_GUID>` 取代成您在步驟 3 - 4 中擷取的值。 

6. 複製儲存在您服務實例中之每個金鑰的 ID 值。

7. 執行下列 cURL 指令，以永久地刪除金鑰及其內容。

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    將 `<access_token>`, `<organization_GUID>`, `<space_GUID>` 及 `<key_ID>` 取代成您在步驟 3 - 5 中擷取的值。請針對每個金鑰重複指令。    

8. 執行下列 cURL 指令，驗證您的金鑰已刪除。

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    將 `<access_token>`, `<organization_GUID>` 及 `<space_GUID>` 取代成您在步驟 3 - 4 中擷取的值。 

9. 刪除 {{site.data.keyword.keymanagementserviceshort}} 服務實例。

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. 選用項目：導覽至 {{site.data.keyword.cloud_notm}} 儀表板，驗證您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例已刪除。

    您也可以執行下列指令，列出目標空間中可用的 Cloud Foundry 服務。

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## 無法存取使用者介面
{: #unable-to-access-ui}

當您存取 {{site.data.keyword.keymanagementserviceshort}} 使用者介面時，無法如預期地載入使用者介面。

從 {{site.data.keyword.cloud_notm}} 儀表板中，選取您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例。
{: tsSymptoms}

您會看到下列錯誤： 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

在 2017 年 12 月 15 日，我們已將新的特性（例如[封套加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)）新增至 {{site.data.keyword.keymanagementserviceshort}} 服務。您現在可以在資源群組裡佈建 {{site.data.keyword.keymanagementserviceshort}} 服務，而不需要指定 Cloud Foundry 組織及空間。
{: tsCauses}

這些變更已影響較舊服務實例的使用者介面。如果您已在 2017 年 9 月 28 日之前建立 {{site.data.keyword.keymanagementserviceshort}} 實例，則使用者介面可能不會如預期地運作。

我們將在我們這端努力修正此問題。暫時解決方案是您可以使用 {{site.data.keyword.keymanagementserviceshort}} API 來繼續管理金鑰。
{: tsResolve}

您可以使用舊式 `https://ibm-key-protect.edge.bluemix.net` 端點以與 {{site.data.keyword.keymanagementserviceshort}} 服務互動。

**要求範例**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## 無法建立或刪除金鑰
{: #unable-to-create-keys}

當您存取 {{site.data.keyword.keymanagementserviceshort}} 使用者介面時，看不到新增或刪除金鑰的選項。

從 {{site.data.keyword.cloud_notm}} 儀表板中，選取您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例。
{: tsSymptoms}

您可以看到金鑰清單，但看不到新增或刪除金鑰的選項。 

您沒有正確的授權可執行 {{site.data.keyword.keymanagementserviceshort}} 動作。
{: tsCauses} 

請向管理者確認已將適當資源群組或服務實例中的正確角色指派給您。如需角色的相關資訊，請參閱[角色及許可權](/docs/services/key-protect?topic=key-protect-manage-access#roles)。
{: tsResolve}

## 取得協助及支援
{: #getting-help}

如果您在使用 {{site.data.keyword.keymanagementserviceshort}} 時有問題或疑問，則可以檢查 {{site.data.keyword.cloud_notm}}，或是搜尋資訊或透過討論區提問來取得協助。您也可以開立支援問題單。
{: shortdesc}

您可以移至[狀態頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/status?tags=platform,runtimes,services)，以檢查 {{site.data.keyword.cloud_notm}} 是否可用。

您可以檢閱討論區，看看是否其他使用者也遇到相同問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.cloud_notm}} 開發團隊能看到它。

- 如果您有 {{site.data.keyword.keymanagementserviceshort}} 的相關技術問題，請將問題張貼在 [Stack Overflow ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window}，並使用 "ibm-cloud" 和 "key-protect" 來標記問題。
- 若是服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} 討論區。請包含 "ibm-cloud" 和 "key-protect" 標籤。

如需使用討論區的詳細資料，請參閱[取得支援 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/get-support?topic=get-support-using-avatar){: new_window}。

如需開立 {{site.data.keyword.IBM_notm}} 支援問題單的相關資訊，或支援層次與問題單嚴重性的相關資訊，請參閱[與支援中心聯絡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}。
