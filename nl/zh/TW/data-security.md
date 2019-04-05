---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: data security, Key Protect compliance, encryption key deletion

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

# 安全與法規遵循
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} 已制定資料安全策略以滿足您的法規遵循需求，並確保您的資料在雲端中保持安全且受到保護。
{: shortdesc}

## 資料安全及加密
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} 使用 [{{site.data.keyword.cloud_notm}} Hardware Security Module (HSM) ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/hardware-security-module) 來產生提供者管理的金鑰資料，並執行[封套加密](/docs/services/key-protect/envelope-encryption.html)作業。HSM 是一種防竄改的硬體裝置，用於儲存及使用加密金鑰資料，而不會將金鑰公開在加密界限之外。

透過 HTTPS 進行服務存取，而內部 {{site.data.keyword.keymanagementserviceshort}} 通訊使用「傳輸層安全 (TLS) 1.2 」通訊協定來加密傳輸中的資料。

若要進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} 如何滿足您的法規遵循需求，請參閱[平台和服務法規遵循](/docs/overview/security.html#compliancetable)。

## 資料刪除
{: #data-deletion}

刪除金鑰之後，服務會將金鑰標示為已刪除，且金鑰會轉移至_已破壞_ 狀態。這個狀態下的金鑰無法再回復，而且使用該金鑰的雲端服務無法再解密與該金鑰相關聯的資料。您的資料會以其加密形式保留在那些服務中。與金鑰相關聯的 meta 資料（例如金鑰轉移歷程及名稱）保存在 {{site.data.keyword.keymanagementserviceshort}} 資料庫中。 

刪除 {{site.data.keyword.keymanagementserviceshort}} 中的金鑰是一項破壞性作業。請牢記，在刪除金鑰之後，您無法回復此動作，而且在刪除金鑰的同時，任何與該金鑰相關聯的資料都會立即遺失。在刪除金鑰之前，請先檢閱與該金鑰相關聯的資料，並確定您不再需要存取此資料。請不要刪除在正式作業環境中正在保護資料的金鑰。 

為了協助您判定金鑰所保護的資料，您可以執行 `ibmcloud iam authorization-policies` 來檢視您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例對映至現有雲端服務的方式。若要進一步瞭解如何從主控台檢視服務授權，請參閱[授與服務之間的存取權](/docs/iam/authorizations.html#serviceauth)。
{: note}
