---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 封套加密
{: #envelope-encryption}

封套加密這種作法，是使用資料加密金鑰 (DEK) 來加密資料，然後使用您可完全管理的主要金鑰來加密 DEK。
{: shortdesc}

{{site.data.keyword.keymanagementservicefull}} 透過進階加密來保護您已儲存的資料，並提供數個優點：

<table>
  <th>優點</th>
  <th>說明</th>
  <tr>
    <td>客戶管理的加密金鑰</td>
    <td>您可以使用此服務來佈建主要金鑰，以保護雲端中已加密資料的安全。主要金鑰作為主要金鑰包裝金鑰，可協助您管理及保護 {{site.data.keyword.cloud_notm}} 資料服務中所佈建的資料加密金鑰 (DEK)。您可以決定匯入現有主要金鑰，還是讓 {{site.data.keyword.keymanagementserviceshort}} 代表您產生它們。</td>
  </tr>
  <tr>
    <td>機密性及完整性保護</td>
    <td>{{site.data.keyword.keymanagementserviceshort}} 以「Galois/計數器模式 (GCM)」使用「進階加密標準 (AES)」演算法來建立及保護金鑰。在服務中建立金鑰時，{{site.data.keyword.keymanagementserviceshort}} 會在 {{site.data.keyword.cloud_notm}} 硬體安全模組 (HSM) 的信任界限內產生它們，因此只有您才有權存取加密金鑰。</td>
  </tr>
  <tr>
    <td>資料的加密清除</td>
    <td>如果您的組織偵測到安全問題，或者您的應用程式不再需要一組資料，則可以選擇從雲端永久地清除資料。當您刪除保護其他 DEK 的主要金鑰時，即可確保無法再存取或解密這些金鑰的相關聯資料。</td>
  </tr>
  <tr>
    <td>委派的使用者存取控制</td>
    <td>{{site.data.keyword.keymanagementserviceshort}} 支援一個集中化存取控制系統，可對您的金鑰啟用精細存取。[透過指派 IAM 使用者角色及進階許可權](/docs/services/keymgmt/keyprotect_manage_access.html#roles)，安全管理者就可以決定誰能存取服務中的哪些主要金鑰。</td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 說明客戶所管理加密的好處</caption>
</table>

## 如何運作
{: #overview}

封套加密結合多個加密演算法的長處，用來保護您在雲端中的機密資料。其運作方式是使用您可完全管理的主要金鑰，以利用進階加密來包裝一個以上的資料加密金鑰 (DEK)。此金鑰包裝處理程序會建立已包裝的 DEK，它們能保護您儲存的資料免於遭受未獲授權的存取或曝光。解除包裝 DEK 會使用相同的主要金鑰來反轉封套加密處理程序，進而產生解密且經過鑑別的資料。
 
下圖顯示金鑰包裝功能的環境定義視圖。
![此圖顯示封套加密的環境定義視圖。](../images/envelope-encryption_min.svg)

「NIST 特殊出版品 800-57」的「金鑰管理建議」中，簡要地論述封套加密。若要進一步瞭解，請參閱 [NIST SP 800-57 Pt. 1 Rev. 4. ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}

## 金鑰類型
{: #key_types}

對於資料的進階加密及管理，服務支援兩種金鑰類型：主要金鑰及標準金鑰。

<dl>
  <dt>主要金鑰</dt>
    <dd>主要金鑰是 {{site.data.keyword.keymanagementserviceshort}} 中的主要資源。它們是對稱金鑰包裝金鑰，用來作為信任主要金鑰，以包裝（加密）及解除包裝（解密）資料服務中所儲存的其他金鑰。使用 {{site.data.keyword.keymanagementserviceshort}}，您可以建立、儲存及管理主要金鑰的生命週期，來完全控制雲端中所儲存的其他金鑰。與標準金鑰不同，主要金鑰永遠無法離開 {{site.data.keyword.keymanagementserviceshort}} 服務的範圍。</dd>
  <dt>標準金鑰</dt>
    <dd>標準金鑰是用於加密的加密金鑰。一般而言，標準金鑰會直接加密資料。使用 {{site.data.keyword.keymanagementserviceshort}}，您可以建立、儲存及管理標準金鑰的生命週期。在服務中匯入或產生標準金鑰之後，即可將它匯出至外部資料資源（例如儲存空間儲存區），以加密機密資訊。加密已儲存資料的標準金鑰稱為資料加密金鑰 (DEK)，其可使用進階加密進行包裝。已包裝的 DEK 不會儲存在 {{site.data.keyword.keymanagementserviceshort}} 中。</dd>
</dl>

在 {{site.data.keyword.keymanagementserviceshort}} 中建立金鑰之後，系統會傳回一個 ID 值，您可以用它來對服務發出 API 呼叫。您可以使用 {{site.data.keyword.keymanagementserviceshort}} GUI 或 [{{site.data.keyword.keymanagementserviceshort}} API](https://console.bluemix.net/apidocs/639) 來擷取金鑰的 ID 值。 

## 包裝金鑰
{: #wrapping}

主要金鑰可協助您分組、管理及保護雲端中所儲存的資料加密金鑰 (DEK)。在 {{site.data.keyword.keymanagementserviceshort}} 中指定您可完全管理的主要金鑰，即可使用進階加密來包裝一個以上的 DEK。 

在 {{site.data.keyword.keymanagementserviceshort}} 中指定主要金鑰之後，即可使用 {{site.data.keyword.keymanagementserviceshort}} API 將金鑰 wrap 要求傳送至服務。金鑰 wrap 作業同時提供 DEK 的機密性及完整性保護。下圖顯示運作中的金鑰包裝處理程序：
![此圖顯示運作中的金鑰包裝。](../images/wrapping-keys_min.svg)

下表說明執行金鑰包裝作業所需的輸入：
<table>
  <th>輸入</th>
  <th>說明</th>
  <tr>
    <td>主要金鑰 ID</td>
    <td>您要用於包裝之主要金鑰的 ID 值。主要金鑰可以匯入至服務，也可以從其 HSM 於 {{site.data.keyword.keymanagementserviceshort}} 中產生。用於包裝的主要金鑰必須是 256、384 或 512 位元，wrap 要求才能成功。</td>
  </tr>
  <tr>
    <td>純文字</td>
    <td>選用項目：DEK 的金鑰資料，包含您要管理及保護的資料。用於金鑰包裝的純文字必須以 base64 編碼。若要產生 256 位元 DEK，您可以省略 `plaintext` 屬性。此服務會產生以 base64 編碼的 DEK 來用於金鑰包裝。</td>
  </tr>
  <tr>
    <td>其他鑑別資料 (AAD)</td>
    <td>選用項目：檢查金鑰內容完整性的字串陣列。每一個字串最多可以保留 255 個字元。如果您在 wrap 要求期間提供 AAD，則必須在後續 unwrap 要求期間指定相同的 AAD。</td>
  </tr>
    <caption style="caption-side:bottom;">表 2. {{site.data.keyword.keymanagementserviceshort}} 中金鑰包裝所需的輸入</caption>
</table>

如果您傳送 wrap 要求，而未指定要加密的純文字，則 AES-GCM 加密演算法會產生純文字並將其轉換為難理解的資料格式（稱為密文）。此處理程序使用新的金鑰資料來輸出 256 位元 DEK。系統接著會使用 AES 金鑰包裝演算法，它會使用指定的主要金鑰來包裝 DEK 及其金鑰資料。成功的 wrap 作業會傳回以 base64 編碼的已包裝 DEK，而您可以將它儲存在 {{site.data.keyword.cloud_notm}} 應用程式或服務中。 

## 解除包裝金鑰
{: #unwrapping}

解除包裝資料加密金鑰 (DEK) 會解密並鑑別金鑰內的內容，並將原始金鑰資料傳回給您的資料服務。 

如果您的商業應用程式需要存取已包裝 DEK 的內容，則可以使用 {{site.data.keyword.keymanagementserviceshort}} API 將 unwrap 要求傳送給服務。若要解除包裝 DEK，請指定主要金鑰的 ID 值以及起始 wrap 要求期間所傳回的 `ciphertext` 值。若要完成 unwrap 要求，您也必須提供其他鑑別資料 (AAD)，以檢查金鑰內容的完整性。

下圖顯示運作中的金鑰解除包裝。
![此圖顯示將資料解除包裝的運作方式。](../images/unwrapping-keys_min.svg)

在您傳送 unwrap 要求之後，系統會使用相同的 AES 演算法來反轉金鑰包裝處理程序。成功的 unwrap 作業會將以 base64 編碼的 `plaintext` 值傳回給 {{site.data.keyword.cloud_notm}} 靜置資料服務。




