---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# 常見問題
{: #faqs}

您可以使用下列常見問題來協助您使用 {{site.data.keyword.keymanagementservicelong}}。

## 定價如何運作於 {{site.data.keyword.keymanagementserviceshort}}？
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} 針對需要 20 個金鑰或更少金鑰的使用者，提供一個無需收費的[累進層級方案](https://{DomainName}/catalog/services/key-protect)。每個 {{site.data.keyword.cloud_notm}} 帳戶可以有 20 個免費金鑰。如果您的團隊需要多個 {{site.data.keyword.keymanagementserviceshort}} 實例，{{site.data.keyword.cloud_notm}} 會在帳戶內的所有實例之間新增您的作用中金鑰，然後套用定價。 

## 何謂作用中加密金鑰？
{: #what-is-active-encryption-key}
{: faq}

當您將加密金鑰匯入至 {{site.data.keyword.keymanagementserviceshort}} 時，或當您使用 {{site.data.keyword.keymanagementserviceshort}} 從其 HSM 產生金鑰時，那些金鑰就變成_作用中_ 金鑰。定價是根據 {{site.data.keyword.cloud_notm}} 帳戶內的所有作用中金鑰。 

## 我應該如何分組並管理金鑰？
{: #how-to-group-keys}
{: faq}

從定價角度來看，使用 {{site.data.keyword.keymanagementserviceshort}} 的最佳方式是建立數量有限的根金鑰，然後使用那些根金鑰來加密由外部應用程式或雲端資料服務所建立的資料加密金鑰。 

若要進一步瞭解如何使用根金鑰來保護資料加密金鑰，請參閱[使用封套加密保護資料](/docs/services/key-protect?topic=key-protect-envelope-encryption)。

## 何謂根金鑰？
{: #what-is-root-key}
{: faq}

根金鑰是 {{site.data.keyword.keymanagementserviceshort}} 中的主要資源。它們是對稱金鑰包裝金鑰，用來作為信任根金鑰，可保護資料服務中儲存的其他金鑰與[封套加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)。使用 {{site.data.keyword.keymanagementserviceshort}}，您可以建立、儲存及管理根金鑰的生命週期，來完全控制雲端中所儲存的其他金鑰。 

## 何謂封套加密？
{: #what-is-envelope-encryption}
{: faq}

封套加密是使用_資料加密金鑰_ 來加密資料的作法，然後利用高度安全的_金鑰包裝金鑰_ 來加密資料加密金鑰。您的資料將在靜置狀態下套用多個加密層來受到保護。若要瞭解如何針對 {{site.data.keyword.cloud_notm}} 資源啟用封套加密，請參閱[整合服務](/docs/services/key-protect?topic=key-protect-integrate-services)。

## 金鑰名稱可以多長？
{: #key-names}
{: faq}

您可以使用長度最多 90 個字元的金鑰名稱。

## 我可以將個人資訊儲存為金鑰的 meta 資料嗎？
{: #personal-data}
{: faq}

為了保護個人資料的機密性，請不要將個人識別資訊 (PII) 儲存為金鑰的 meta 資料。個人資訊包括您的姓名、地址、電話號碼、電子郵件位址，或者可識別、聯絡或找到您、您的客戶或其他人的其他資訊。


您要負責確保所有您儲存為 {{site.data.keyword.keymanagementserviceshort}} 資源及加密金鑰的 meta 資料的資訊安全。如需個人資料的更多範例，請參閱 [NIST 特殊出版品 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external} 的第 2.2 節。
{: important}

## 可以在其他地區使用某個地區中建立的金鑰嗎？
{: #keys-across-regions}
{: faq}

您的加密金鑰僅限於您在其中建立它們的地區。{{site.data.keyword.keymanagementserviceshort}} 不會將加密金鑰複製或匯出到其他地區。

## 如何控制誰有權存取金鑰？
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} 支援由 {{site.data.keyword.iamlong}} 控管的集中式存取控制系統，可協助您管理使用者和加密金鑰的存取。
如果您是服務的安全管理者，則可以指派[對應至您想要授與給團隊成員的特定 {{site.data.keyword.keymanagementserviceshort}} 許可權的 Cloud IAM 角色](/docs/services/key-protect?topic=key-protect-manage-access#roles)。

## 如何監視對 {{site.data.keyword.keymanagementserviceshort}} 發出的 API 呼叫？
{: faq}

您可以使用 {{site.data.keyword.cloudaccesstrailfull_notm}} 服務來追蹤使用者及應用程式與 {{site.data.keyword.keymanagementserviceshort}} 服務實例的互動情況。例如，當您在 {{site.data.keyword.keymanagementserviceshort}} 中建立、匯入、刪除或讀取金鑰時，會產生 {{site.data.keyword.cloudaccesstrailshort}} 事件。這些事件會自動轉遞至與 {{site.data.keyword.keymanagementserviceshort}} 服務佈建所在地區相同之地區的 {{site.data.keyword.cloudaccesstrailshort}} 服務。

若要進一步瞭解，請參閱 [Activity Tracker 事件](/docs/services/key-protect?topic=key-protect-at-events)。

## 刪除金鑰時會發生什麼情況？
{: #key-destruction}
{: faq}

刪除金鑰之後，服務會將金鑰標示為已刪除，且金鑰會轉移至_已破壞_ 狀態。這個狀態下的金鑰無法再回復，而且使用該金鑰的雲端服務無法再解密與該金鑰相關聯的資料。您的資料會以其加密形式保留在那些服務中。與金鑰相關聯的 meta 資料（例如金鑰轉移歷程及名稱）保存在 {{site.data.keyword.keymanagementserviceshort}} 資料庫中。 

在您刪除金鑰之前，請確定不再需要存取任何與金鑰相關聯的資料。這個動作無法回復。

## 需要取消佈建服務實例時會發生什麼情況？
{: #deprovision-service}
{: faq}

如果您決定從 {{site.data.keyword.keymanagementserviceshort}} 繼續，則必須在可以取消佈建服務之前，先刪除服務實例中所有剩餘的金鑰。刪除服務實例之後，{{site.data.keyword.keymanagementserviceshort}} 會使用[封套加密](/docs/services/key-protect?topic=key-protect-envelope-encryption)來加密/解構與服務實例相關聯的任何資料。 

