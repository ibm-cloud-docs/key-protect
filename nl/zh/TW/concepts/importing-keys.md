---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# 自帶加密金鑰到雲端
{: #importing-keys}

加密金鑰包含資訊子集，例如可協助您識別金鑰的 meta 資料，以及用來加密及解密資料的_金鑰資料_。
{: shortdesc}

當您使用 {{site.data.keyword.keymanagementserviceshort}} 來建立金鑰時，服務會代表您產生根植於雲端型硬體安全模組 (HSM) 中的加密金鑰資料。然而取決於您的商業需求，您可能需要從內部解決方案產生金鑰資料，然後藉由將金鑰匯入至 {{site.data.keyword.keymanagementserviceshort}} 來將內部部署金鑰管理基礎架構擴充至雲端。

<table>
  <th>優點</th>
  <th>說明</th>
  <tr>
    <td>自帶金鑰 (BYOK) </td>
    <td>您想要從內部部署的硬體安全模組 (HSM) 產生強度大的金鑰，以完全控制及加強金鑰管理實務。如果您選擇從內部金鑰管理基礎架構匯出對稱金鑰，您可以使用 {{site.data.keyword.keymanagementserviceshort}} 將它們安全地帶到雲端。</td>
  </tr>
  <tr>
    <td>安全匯入根金鑰資料</td>
    <td>將金鑰匯出至雲端時，您想要保證金鑰資料在進行時受到保護。<a href="#transport-keys">使用傳輸金鑰</a>來減輕攔截式攻擊，以安全地將根金鑰資料匯入至您的 {{site.data.keyword.keymanagementserviceshort}} 服務實例。</td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 說明匯入金鑰資料的好處</caption>
</table>


## 提前規劃匯入金鑰資料
{: #plan-ahead}

當您備妥要將根金鑰資料匯入至服務時，請記住下列考量事項。

<dl>
  <dt>檢閱用來建立金鑰資料的選項</dt>
    <dd>根據您的安全需求，探索建立 256 位元對稱加密金鑰的選項。例如，您可以使用由 FIPS 驗證的內部部署硬體安全模組 (HSM) 所支援的內部金鑰管理系統，先產生金鑰資料然後自帶金鑰到雲端。如果您要建置一個概念證明，也可以使用加密工具箱（例如 <a href="https://www.openssl.org/" target="_blank">OpenSSL</a>），針對您的測試需要產生可匯入至 {{site.data.keyword.keymanagementserviceshort}} 的金鑰資料。</dd>
  <dt>選擇選項以將金鑰資料匯入至 {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>從兩個選項中選擇，以根據您的環境或工作負載所需的安全層次來匯入根金鑰。依預設，在使用「傳輸層安全 (TLS) 」 1.2 通訊協定進行傳輸時，{{site.data.keyword.keymanagementserviceshort}} 會加密您的金鑰資料。如果您是第一次建置概念證明或嘗試服務，您可以使用此預設選項，將根金鑰資料匯入至 {{site.data.keyword.keymanagementserviceshort}}。如果您的工作負載需要 TLS 以外的安全機制，您也可以<a href="#transport-keys">使用傳輸金鑰</a>來加密根金鑰資料並將其匯入至服務中。</dd>
  <dt>提前規劃加密金鑰資料</dt>
    <dd>如果您選擇使用傳輸金鑰來加密金鑰資料，請判定在金鑰資料上執行 RSA 加密的方法。您必須使用 <a href="https://tools.ietf.org/html/rfc3447" target="_blank">PKCS #1 2.1 版 RSA 加密標準</a>所指定的 <code>RSAES_OAEP_SHA_256</code> 加密方法。請檢閱內部金鑰管理系統或內部部署 HSM 的功能，以判定您的選項。</dd>
  <dt>管理匯入金鑰資料的生命週期</dt>
    <dd>在您將金鑰資料匯入至服務之後，請牢記，您要負責管理金鑰的完整生命週期。使用 {{site.data.keyword.keymanagementserviceshort}} API，您可以在決定將金鑰上傳至服務時，設定該金鑰的到期日。不過，如果您要<a href="/docs/services/key-protect?topic=key-protect-rotate-keys">替換匯入的根金鑰</a>，則必須產生並提供新的金鑰資料來淘汰並取代現有金鑰。</dd>
</dl>

## 使用傳輸金鑰
{: #transport-keys}

傳輸金鑰目前是測試版特性。測試版特性隨時可能會變更，未來更新項目可能會帶來一些變更，而這些變更與最新版本並不相容。
{: important}

如果您要在將金鑰資料匯入至 {{site.data.keyword.keymanagementserviceshort}} 之前予以加密，您可以使用 {{site.data.keyword.keymanagementserviceshort}} API，為服務實例建立一個傳輸加密金鑰。 

傳輸金鑰是 {{site.data.keyword.keymanagementserviceshort}} 中的一種資源類型，可讓根金鑰資料安全匯入您的服務實例。在內部部署時使用傳輸金鑰來加密金鑰資料，您可以根據您指定的原則，在根金鑰傳輸至 {{site.data.keyword.keymanagementserviceshort}} 時保護它們。例如，您可以對傳輸金鑰設定一項原則，以根據時間和使用量計數來限制金鑰的使用。

### 如何運作
{: #how-transport-keys-work}

當您為服務實例[建立傳輸金鑰](/docs/services/key-protect?topic=key-protect-create-transport-keys)時，{{site.data.keyword.keymanagementserviceshort}} 會產生一個 4096 位元的 RSA 金鑰。此服務會加密私密金鑰，然後傳回公開金鑰和可用於加密及匯入根金鑰資料的匯入記號。 

當您備妥要[將根金鑰匯入](/docs/services/key-protect?topic=key-protect-import-root-keys#api)至 {{site.data.keyword.keymanagementserviceshort}} 時，您會提供加密的根金鑰資料，以及用來驗證公開金鑰完整性的匯入記號。為了完成要求，{{site.data.keyword.keymanagementserviceshort}} 會使用與服務實例相關聯的私密金鑰，以解密已加密的根金鑰資料。這項程序可確保只有在 {{site.data.keyword.keymanagementserviceshort}} 中產生的傳輸金鑰可以解密您在服務中匯入及儲存的金鑰資料。

每個服務實例只能建立一個傳輸金鑰。若要進一步瞭解傳輸金鑰的擷取限制，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。
{: note} 

### API 方法
{: #secure-import-api-methods}

{{site.data.keyword.keymanagementserviceshort}} API 會在幕後驅動傳輸金鑰建立處理程序。  

下表列出的 API 方法會設定一個鎖定器，並為服務實例建立傳輸金鑰。

<table>
  <tr>
    <th>方法</th>
    <th>說明</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">建立傳輸金鑰</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">擷取傳輸金鑰 meta 資料</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">擷取傳輸金鑰和匯入記號</a></td>
  </tr>
  <caption style="caption-side:bottom;">表 2. 說明 {{site.data.keyword.keymanagementserviceshort}} API 方法</caption>
</table>

若要進一步瞭解如何在 {{site.data.keyword.keymanagementserviceshort}} 中以程式設計方式管理您的金鑰，請參閱 [{{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。


## 下一步為何？

- 若要瞭解如何為服務實例建立傳輸金鑰，請參閱[建立傳輸金鑰](/docs/services/key-protect?topic=key-protect-create-transport-keys)。
- 若要進一步瞭解如何將金鑰匯入至服務，請參閱[匯入根金鑰](/docs/services/key-protect?topic=key-protect-import-root-keys)。 
