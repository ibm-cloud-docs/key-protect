---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

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

# 疑難排解
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} 的一般使用問題可能包括在您與 API 互動時提供正確的標頭或認證。在許多情況下，您都可以遵循一些簡單的步驟來回復這些問題。
{: shortdesc}

## 無法存取使用者介面
{: #unable-to-access-ui}

當您存取 {{site.data.keyword.keymanagementserviceshort}} 使用者介面時，無法如預期載入使用者介面。

從 {{site.data.keyword.cloud_notm}} 儀表板中，選取您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例。
{: tsSymptoms}

您會看到下列錯誤： 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

在 2017 年 12 月 15 日，我們已將新的特性（例如[封套加密](/docs/services/key-protect/concepts/envelope-encryption.html)）新增至 {{site.data.keyword.keymanagementserviceshort}} 服務。您現在可以廣域地佈建 {{site.data.keyword.keymanagementserviceshort}} 服務，而不需要指定 Cloud Foundry 組織及空間。
{: tsCauses}

這些變更已影響較舊服務實例的使用者介面。如果您已在 2017 年 9 月 28 日之前建立 {{site.data.keyword.keymanagementserviceshort}} 實例，則使用者介面可能不會如預期運作。

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

請向管理者確認已將適當資源群組或服務實例中的正確角色指派給您。如需角色的相關資訊，請參閱[角色及許可權](/docs/services/key-protect/manage-access.html#roles)。
{: tsResolve}

## 取得協助及支援
{: #getting-help}

如果您在使用 {{site.data.keyword.keymanagementserviceshort}} 時有問題或疑問，則可以檢查 {{site.data.keyword.cloud_notm}}，或是搜尋資訊或透過討論區提問來取得協助。您也可以開立支援問題單。
{: shortdesc}

您可以移至[狀態頁面 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/status?tags=platform,runtimes,services)，以檢查 {{site.data.keyword.cloud_notm}} 是否可用。

您可以檢閱討論區，看看是否其他使用者也遇到相同問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.cloud_notm}} 開發團隊能看到它。

- 如果您有 {{site.data.keyword.keymanagementserviceshort}} 的相關技術問題，請將問題張貼在 [Stack Overflow ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window}，並使用 "ibm-cloud" 和 "key-protect" 來標記問題。
- 若是服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} 討論區。請包含 "ibm-cloud" 和 "key-protect" 標籤。

如需使用討論區的詳細資料，請參閱[取得協助 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}。

如需開立 {{site.data.keyword.IBM_notm}} 支援問題單的相關資訊，或支援層次與問題單嚴重性的相關資訊，請參閱[與支援中心聯絡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}。
