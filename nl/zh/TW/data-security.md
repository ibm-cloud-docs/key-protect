---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# 安全與法規遵循
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} 已制定資料安全策略以滿足您的法規遵循需求，並確保您的資料在雲端中保持安全且受到保護。
{: shortdesc}

## 安全性就緒狀態
{: #security-ready}

透過針對系統、網路與安全設計繼承 {{site.data.keyword.IBM_notm}} 的最佳作法，{{site.data.keyword.keymanagementserviceshort}} 會確保安全性就緒狀態。 

若要進一步瞭解整個 {{site.data.keyword.cloud_notm}} 的安全性控制，請參閱[如何知道我的資料是否安全？](/docs/overview?topic=overview-security#security)。
{: tip}

### 資料加密
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} 會使用[{{site.data.keyword.cloud_notm}} Hardware Security Module (HSM)](https://www.ibm.com/cloud/hardware-security-module){: external} 來產生提供者管理的金鑰資料並執行[封套加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)作業。HSM 是一種防竄改的硬體裝置，用於儲存及使用加密金鑰資料，而不會將金鑰公開在加密界限之外。

透過 HTTPS 進行服務存取，而內部 {{site.data.keyword.keymanagementserviceshort}} 通訊使用「傳輸層安全 (TLS) 1.2 」通訊協定來加密傳輸中的資料。

### 資料刪除
{: #data-deletion}

刪除金鑰之後，服務會將金鑰標示為已刪除，且金鑰會轉移至_已破壞_ 狀態。這個狀態下的金鑰無法再回復，而且使用該金鑰的雲端服務無法再解密與該金鑰相關聯的資料。您的資料會以其加密形式保留在那些服務中。與金鑰相關聯的 meta 資料（例如金鑰轉移歷程及名稱）保存在 {{site.data.keyword.keymanagementserviceshort}} 資料庫中。 

刪除 {{site.data.keyword.keymanagementserviceshort}} 中的金鑰是一項破壞性作業。請牢記，在刪除金鑰之後，您無法回復此動作，而且在刪除金鑰的同時，任何與該金鑰相關聯的資料都會立即遺失。在刪除金鑰之前，請先檢閱與該金鑰相關聯的資料，並確定您不再需要存取此資料。請不要刪除在正式作業環境中正在保護資料的金鑰。 

為了協助您判定金鑰所保護的資料，您可以執行 `ibmcloud iam authorization-policies` 來檢視您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例對映至現有雲端服務的方式。若要進一步瞭解如何從主控台檢視服務授權，請參閱[授與服務之間的存取權](/docs/iam?topic=iam-serviceauth)。
{: note}

## 法規遵循就緒狀態
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} 符合對廣域、產業與區域法規遵循標準的控制，包含 GDPR、HIPAA 與 ISO 27001/27017/27018 以及其他。 

如需 {{site.data.keyword.cloud_notm}} 法規遵循憑證的完整清單，請參閱 [{{site.data.keyword.cloud_notm}} 上的法規遵循](https://www.ibm.com/cloud/compliance){: external}。
{: tip}

### 歐盟支援
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} 正好有額外的控制以保護歐盟 (EU) 中的 {{site.data.keyword.keymanagementserviceshort}} 資源。 

若您使用德國法蘭克福地區的 {{site.data.keyword.keymanagementserviceshort}} 資源來處理歐洲居民的個人資料，您可以啟用 {{site.data.keyword.cloud_notm}} 帳戶的歐盟支援設定。若要進一步瞭解相關資訊，請參閱[啟用歐盟支援設定](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)與[要求支援歐盟中的資源](/docs/get-support?topic=get-support-getting-customer-support#eusupported)。

### 一般資料保護法規 (GDPR)
{: #gdpr}

GDPR 在整個歐盟範圍內探查並建立統一的資料保護法架構，旨在將居民的個人資料控制權歸還給所屬個人，同時對世界各地管理及處理這些資料的其他人施行嚴格的規則加以規定。

{{site.data.keyword.IBM_notm}} 已確認將創新資料隱私權、安全性，與管理解決方案提供給用戶端與 {{site.data.keyword.IBM_notm}} 事業夥伴以協助 GDPR 就緒狀態的進程。

為了確保 {{site.data.keyword.keymanagementserviceshort}} 資源的 GDPR 法規遵循，請為您的 {{site.data.keyword.cloud_notm}} 帳戶[啟用歐盟支援設定](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)。您可以進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} 如何透過檢閱下列附錄來處理與保護個人資料的相關資訊。

- [{{site.data.keyword.keymanagementservicefull_notm}} 資料表附錄 (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} 資料處理附錄 (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA 支援
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} 符合美國醫療保險轉移和責任法 (HIPAA) 的控制以確保對受保護的健康資訊 (PHI) 提供防衛措施。 

若您與公司均為由 HIPAA 所定義的受管轄機構，則您可以對 {{site.data.keyword.cloud_notm}} 帳戶啟用 HIPPA 支援設定。若要進一步瞭解相關資訊，請參閱[啟用 HIPAA 支援設定](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa)。

### ISO 27001、27017、27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} 已受到 ISO 27001、27017 與 27018 的認證。透過造訪 [{{site.data.keyword.cloud_notm}} 上的法規遵循](https://www.ibm.com/cloud/compliance){: external}，您可以檢視法規遵循憑證。 

### SOC 2 類型 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} 已受到 SOC 2 類型 1 的認證。如需要求 {{site.data.keyword.cloud_notm}} SOC 2 報告的相關資訊，請參閱 [{{site.data.keyword.cloud_notm}} 上的法規遵循](https://www.ibm.com/cloud/compliance){: external}。
