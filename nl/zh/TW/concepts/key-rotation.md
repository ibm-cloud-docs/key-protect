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

# 金鑰替換
{: #key-rotation}

當您將加密金鑰淘汰，並產生新的加密金鑰資料時，即會發生金鑰替換。

定期替換金鑰有助於您符合業界標準和加密最佳作法。下表說明金鑰替換的主要好處：

<table>
  <th>優點</th>
  <th>說明</th>
  <tr>
    <td>金鑰的加密期間管理</td>
    <td>金鑰替換限制了受單一金鑰保護的資訊量。藉由定期替換主要金鑰，您也縮短了金鑰的加密期間。加密金鑰的生命期限越長，安全侵害的機率便越高。</td>
  </tr>
  <tr>
    <td>突發事件降低</td>
    <td>如果您的組織偵測到安全問題，您可以立即替換金鑰以便降低或減少與金鑰洩漏相關聯的成本。</td>
  </tr>

  <caption style="caption-side:bottom;">表 1. 說明金鑰替換的好處</caption>
</table>

「NIST 特殊出版品 800-57」的「金鑰管理建議」中，論述了金鑰替換。若要進一步瞭解，請參閱 [NIST SP 800-57 Pt. 1 Rev. 4. ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}
{: tip}

## 金鑰替換的運作方式
{: #how-rotation-works}

加密金鑰在其生命期限內，會在不同的[金鑰狀態](/docs/services/key-protect/concepts/key-states.html)間轉移。在_作用中_ 狀態，金鑰會加密及解密資料。在_取消啟動_ 狀態，金鑰無法加密資料，但仍然可以用來進行解密。在_已破壞_ 狀態，金鑰無法再用於加密或解密。

金鑰替換的運作是將金鑰資料安全地從_作用中_ 狀態轉移為_取消啟動_ 狀態。為了取代已淘汰的金鑰資料，新的金鑰資料會移入_作用中_ 狀態，並且可用於加密作業。

在 {{site.data.keyword.keymanagementserviceshort}} 中，您可以隨需應變替換您的主要金鑰，而不需要追蹤已淘汰的主要金鑰資料。下圖顯示金鑰替換功能的環境定義視圖。
![此圖顯示金鑰替換的環境定義視圖。](../images/key-rotation_min.svg)

替換只適用於主要金鑰。
{: note}

### 替換主要金鑰
{: #rotating-key}

針對每個替換要求，{{site.data.keyword.keymanagementserviceshort}} 會將新的金鑰資料與您的主要金鑰相關聯。 

![此圖顯示主要金鑰堆疊的微小視圖。](../images/root-key-stack_min.svg)

替換完成之後，新的主要金鑰資料便可用於使用[封套加密](/docs/services/key-protect/concepts/envelope-encryption.html)保護未來的資料加密金鑰 (DEK)。已淘汰的金鑰資料會移到_取消啟動_ 狀態，只能用來解除包裝及存取較舊且尚未被最新主要金鑰資料保護的 DEK。如果 {{site.data.keyword.keymanagementserviceshort}} 偵測到您正在使用已淘汰的主要金鑰資料解除包裝較舊的 DEK，服務會自動重新加密 DEK 並傳回已包裝且根據最新主要金鑰資料的資料加密金鑰 (WDEK)。請儲存並使用新的 WDEK 來進行未來的 unwrap 作業，以便用最新的主要金鑰資料保護 DEK。

若要了解如何使用 {{site.data.keyword.keymanagementserviceshort}} API 來替換您的主要金鑰，請參閱[替換金鑰](/docs/services/key-protect/rotate-keys.html)。

當您在 {{site.data.keyword.keymanagementserviceshort}} 替換金鑰時，不會向您收取額外的費用。您可以繼續用已淘汰的金鑰資料來對已包裝的資料加密金鑰 (WDEK) 進行解除包裝，而不需額外的成本。如需定價選項的相關資訊，請參閱 [{{site.data.keyword.keymanagementserviceshort}} 型錄頁面](https://{DomainName}/catalog/services/key-protect)。
{: tip}

## 金鑰替換的頻率
{: #rotation-frequency}

在 {{site.data.keyword.keymanagementserviceshort}} 產生主要金鑰之後，您要決定替換的頻率。由於人事異動、處理程序故障，或是組織內部金鑰到期原則的緣故，您可能會想要替換金鑰。 

請定期替換金鑰，例如每 30 天一次，以符合加密最佳作法。針對每個主要金鑰，{{site.data.keyword.keymanagementserviceshort}} 允許每小時替換一次。
