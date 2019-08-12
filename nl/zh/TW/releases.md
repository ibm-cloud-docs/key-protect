---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# 新增功能
{: #releases}

掌握最新的 {{site.data.keyword.keymanagementservicefull}} 可用新增特性。
{: shortdesc}

## 2019 年 6 月
{: #june-2019}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了對 {{site.data.keyword.at_full_notm}} 的支援
{: #added-at-logdna-support}
新增日期：2019-06-22

透過使用 {{site.data.keyword.at_full_notm}}，您現在可以監視 {{site.data.keyword.keymanagementserviceshort}} 服務的 API 呼叫。 

若要進一步瞭解監視 {{site.data.keyword.keymanagementserviceshort}} 活動的相關資訊，請參閱[Activity Tracker 事件](/docs/services/key-protect?topic=key-protect-at-events)。

## 2019 年 5 月
{: #may-2019}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 將 HSM 升級至 FIPS 140-2 Level 3
{: #upgraded-hsms}
新增日期：2019-05-22

{{site.data.keyword.keymanagementserviceshort}} 現在使用{{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 以加密儲存空間與作業。您的 {{site.data.keyword.keymanagementserviceshort}} 金鑰是以 FIPS 140-2 Level 3 標準儲存在防竄改硬體中（適用於所有地區）。 

若要進一步瞭解 {{site.data.keyword.cloud_notm}} HSM 7.0 特性與優點的相關資訊，請查閱[產品頁面](https://www.ibm.com/cloud/hardware-security-module){: external}。

### 結束支援：Cloud Foundry 型 {{site.data.keyword.keymanagementserviceshort}} 服務實例
{: #legacy-service-eol}
新增日期：2019-05-15

根據 Cloud Foundry 的舊式 {{site.data.keyword.keymanagementserviceshort}} 服務已於 2019 年 5 月 15 日結束支援。不再支援 Cloud Foundry 管理的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，並且將不再提供舊式服務的更新項目。建議客戶使用 IAM 管理的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，以便使用服務的最新特性。

若您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例是在 2017 年 12 月 15 日之後建立，則為 IAM 管理的服務實例且不會受到此變更的影響。若有其他問題，請聯絡 Terry Mosbaugh ([mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com)) 以取得協助。

需要從 {{site.data.keyword.cloud_notm}} 資源清單的 **Cloud Foundry 服務**區段移除 {{site.data.keyword.keymanagementserviceshort}} 服務實例嗎？您可以使用[支援中心](https://{DomainName}/unifiedsupport/cases/add)聯絡我們，方法是從您的主控台視圖中提交移除項目的要求。
{: tip}

## 2019 年 3 月
{: #mar-2019}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了原則型金鑰替換的支援
{: #added-support-policy-key-rotation}
新增日期：2019-03-22

您現在可以使用 {{site.data.keyword.keymanagementserviceshort}} 來建立替換原則與根金鑰的關聯。

如需相關資訊，請參閱[設定替換原則](/docs/services/key-protect?topic=key-protect-set-rotation-policy)。若要進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} 中的金鑰替換選項，請參閱[比較金鑰替換選項](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了傳輸金鑰的測試版支援
新增日期：2019-03-20

透過建立 {{site.data.keyword.keymanagementserviceshort}} 服務的傳輸加密金鑰，讓加密金鑰安全匯入雲端。

如需相關資訊，請參閱[自帶加密金鑰到雲端](/docs/services/key-protect?topic=key-protect-importing-keys)。

傳輸金鑰目前是測試版特性。測試版特性隨時可能會變更，未來更新項目可能會帶來一些變更，而這些變更與最新版本並不相容。
{: important}

## 2019 年 2 月
{: #feb-2019}

### 已變更：舊式 {{site.data.keyword.keymanagementserviceshort}} 服務實例
{: #changed-legacy-service-instances}

新增日期：2019-02-13

在 2017 年 12 月 15 日之前佈建的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，是在基於 Cloud Foundry 的舊式基礎架構上執行。此舊式 {{site.data.keyword.keymanagementserviceshort}} 服務將於 2019 年 5 月 15 日解除任務。

**這對您來說有何意義？**

如果您在較舊的 {{site.data.keyword.keymanagementserviceshort}} 服務實例中具有作用中的正式作業金鑰，請確定您將金鑰移轉到 2019 年 5 月 15 日的新服務實例，以避免失去已加密資料的存取權。藉由從 [{{site.data.keyword.cloud_notm}} 主控台](https://{DomainName}/)導覽至資源清單，您可以進行檢查來查看是否您正在使用舊式實例。如果 {{site.data.keyword.keymanagementserviceshort}} 服務實例列在 {{site.data.keyword.cloud_notm}} 資源清單的 **Cloud Foundry 服務**區段中，或者如果您是使用 `https://ibm-key-protect.edge.bluemix.net` API 端點來設定服務目標作業，則會使用 {{site.data.keyword.keymanagementserviceshort}} 的舊式實例。在 2019 年 5 月 15 日之後，無法再存取舊式端點，您將無法設定作業的目標服務。

需要協助來將加密金鑰移轉至新的服務實例嗎？如需詳細步驟，請查閱 [GitHub 中的移轉用戶端](https://github.com/IBM-Cloud/kms-migration-client){: external}。如果您對金鑰的狀態或移轉程序有其他問題，請透過 [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com) 連繫 Terry Mosbaugh。
{: tip}

## 2018 年 12 月
{: #dec-2018}

### 已變更：{{site.data.keyword.keymanagementserviceshort}} API 端點
{: #changed-api-endpoints}

新增日期：2018-12-19

為了與 {{site.data.keyword.cloud_notm}} 的新一致體驗密切配合，{{site.data.keyword.keymanagementserviceshort}} 已更新其服務 API 的基本 URL。

您現在可以更新應用程式，以參照新的 `cloud.ibm.com` 端點。

- `keyprotect.us-south.bluemix.net` 現在是 `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` 現在是 `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` 現在是 `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` 現在是 `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` 現在是 `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` 現在是 `jp-tok.kms.cloud.ibm.com` 

此時支援每個地區服務端點的 URL。 

## 2018 年 10 月
{: #oct-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到東京地區
{: #added-tokyo-region}

新增日期：2018-10-31

您現在可以在東京地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect?topic=key-protect-regions)。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式
{: #added-cli-plugin}

新增日期：2018-10-02

您現在可以使用 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式，以在 {{site.data.keyword.keymanagementserviceshort}} 服務實例中管理金鑰。

若要了解如何安裝外掛程式，請參閱[設定 CLI](/docs/services/key-protect?topic=key-protect-set-up-cli)。若要進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} CLI，[請參閱 CLI 參考資料文件](/docs/services/key-protect?topic=key-protect-cli-reference)。

## 2018 年 9 月
{: #sept-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了對隨需應變金鑰替換的支援
{: #added-key-rotation}

新增日期：2018-09-28

您現在可以使用 {{site.data.keyword.keymanagementserviceshort}}，隨需應變地替換您的根金鑰。

如需相關資訊，請參閱[替換金鑰](/docs/services/key-protect?topic=key-protect-rotate-keys)。

### 已新增：雲端應用程式的端對端安全指導教學
{: #added-security-tutorial}

新增日期：2018-09-14

在尋找程式碼範例以協助您用自己的加密金鑰將儲存空間儲存區內容加密？

您現在可以遵循[新的指導教學](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)，練習為雲端應用程式新增端對端安全。

如需相關資訊，請查閱[GitHub 中的範例應用程式](https://github.com/IBM-Cloud/secure-file-storage){: external}。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到華盛頓特區地區
{: #added-wdc-region}

新增日期：2018-09-10

您現在可以在華盛頓特區地區中建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2018 年 8 月
{: #aug-2018}

### 已變更：{{site.data.keyword.keymanagementserviceshort}} API 文件 URL
{: #changed-api-doc-url}

新增日期：2018-08-28

{{site.data.keyword.keymanagementserviceshort}} API 參考資料已移動！ 

您現在可以在下列網址存取 API 文件：
https://{DomainName}/apidocs/key-protect. 

## 2018 年 3 月
{: #mar-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到法蘭克福地區
{: #added-frankfurt-region}

新增日期：2018-03-21

您現在可以在法蘭克福地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2018 年 1 月
{: #jan-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到雪梨地區
{: #added-sydney-region}

新增日期：2018-01-31

您現在可以在雪梨地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect?topic=key-protect-regions)。

## 2017 年 12 月
{: #dec-2017}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增對「自攜金鑰 (BYOK)」的支援
{: #added-byok-support}

新增日期：2017-12-15

{{site.data.keyword.keymanagementserviceshort}} 現在支援「自攜金鑰 (BYOK)」及客戶管理的加密

- 已引進[根金鑰](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types)，也稱為「客戶根金鑰 (CRK)」，作為服務中的主要資源。 
- 已啟用 {{site.data.keyword.cos_full_notm}} 儲存區的[封套加密](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how)。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到倫敦地區
{: #added-london-region}

新增日期：2017-12-15

現在可以在倫敦地區使用 {{site.data.keyword.keymanagementserviceshort}}。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect?topic=key-protect-regions)。

### 已變更：{{site.data.keyword.iamshort}} 角色
{: #changed-iam-roles}

新增日期：2017-12-15

{{site.data.keyword.iamshort}} 角色已變更，這些角色決定您可以對 {{site.data.keyword.keymanagementserviceshort}} 資源執行的動作。

- `Administrator` 現在是 `Manager`
- `Editor` 現在是 `Writer`
- `Viewer` 現在是 `Reader`

如需相關資訊，請參閱[管理使用者存取](/docs/services/key-protect?topic=key-protect-manage-access)。

## 2017 年 9 月
{: #sept-2017}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了對 Cloud IAM 的支援
{: #added-iam-support}

新增日期：2017-09-19

您現在可以使用 {{site.data.keyword.iamshort}} 來設定和管理 {{site.data.keyword.keymanagementserviceshort}} 資源的存取原則。

如需相關資訊，請參閱[管理使用者存取](/docs/services/key-protect?topic=key-protect-manage-access)。
