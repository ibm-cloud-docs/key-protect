---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# 疑難排解
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} 的一般使用問題可能包括在您與 API 互動時提供正確的標頭或認證。在許多情況下，您都可以遵循一些簡單的步驟來回復這些問題。
{: shortdesc}

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

您可以移至[狀態頁面](https://{DomainName}/status?tags=platform,runtimes,services){: external}，檢查是否可以使用 {{site.data.keyword.cloud_notm}}。

您可以檢閱討論區，看看是否其他使用者也遇到相同問題。使用討論區提問時，請標記您的問題，以便 {{site.data.keyword.cloud_notm}} 開發團隊能看到它。

- 若您有 {{site.data.keyword.keymanagementserviceshort}} 的相關技術問題，請將問題張貼在 [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external}，並使用 "ibm-cloud" 和 "key-protect" 來標記問題。
- 若為服務及開始使用指示的相關問題，請使用 [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external} 討論區。請包含 "ibm-cloud" 和 "key-protect" 標籤。

如需使用討論區的詳細資料，請參閱[取得支援](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external}。

如需開立 {{site.data.keyword.IBM_notm}} 支援問題單的相關資訊，或支援層次與問題單嚴重性的相關資訊，請參閱[與支援中心聯絡](/docs/get-support?topic=get-support-getting-customer-support){: external}。
