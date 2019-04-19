---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: key management service, kms, manage encryption keys, data encryption, data-at-rest, protect data encryption keys

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

# 入門指導教學
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} 可協助您在 {{site.data.keyword.cloud_notm}} 服務之間為應用程式佈建加密金鑰。本指導教學示範如何使用 {{site.data.keyword.keymanagementserviceshort}} 儀表板建立及新增現有加密金鑰，以從一個集中位置來管理資料加密。
{: shortdesc}

## 開始使用加密金鑰
{: #get-started-keys}

從 {{site.data.keyword.keymanagementserviceshort}} 儀表板中，您可以建立加密用的新金鑰，或匯入現有的金鑰。 

從兩種金鑰類型中進行選擇：

<dl>
  <dt>根金鑰</dt>
    <dd>根金鑰是您在 {{site.data.keyword.keymanagementserviceshort}} 中完全管理的對稱金鑰包裝金鑰。您可以使用根金鑰，以利用進階加密來保護其他加密金鑰。若要進一步瞭解，請參閱<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">使用封套加密保護資料</a>。</dd>
  <dt>標準金鑰</dt>
    <dd>標準金鑰是用於加密的對稱金鑰。您可以使用標準金鑰來直接加密及解密資料。</dd>
</dl>

## 建立新金鑰
{: #create-keys}

[建立 {{site.data.keyword.keymanagementserviceshort}} 實例之後](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps)，您便可以在服務中指定金鑰。 

請完成下列步驟，以建立第一個加密金鑰。 

1. 在應用程式詳細資料頁面上，按一下**管理** &gt; **新增金鑰**。
2. 若要建立新的金鑰，請選取**建立金鑰**視窗。

    指定金鑰的詳細資料：

    <table>
      <tr>
        <th>設定</th>
        <th>說明</th>
      </tr>
      <tr>
        <td>名稱</td>
        <td>
          <p>方便識別金鑰且人類可閱讀的唯一別名。</p>
          <p>若要保護您的隱私權，請確定金鑰名稱未包含個人識別資訊 (PII)（例如您的姓名或位置）。</p>
        </td>
      </tr>
      <tr>
        <td>金鑰類型</td>
        <td>您要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">金鑰類型</a>。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. <b>建立金鑰</b>設定的說明</caption>
    </table>

3. 當您填寫完金鑰的詳細資料時，請按一下**建立金鑰**以便確認。 

在服務中所建立的金鑰是 AES-GCM 演算法所支援的對稱 256 位元金鑰。為了提高安全，金鑰是由位於安全 {{site.data.keyword.cloud_notm}} 資料中心的 FIPS 140-2 Level 2 認證硬體安全模組 (HSM) 所產生。 

## 匯入自己的金鑰
{: #import-keys}

將現有金鑰引進到服務中，即可啟用「自帶金鑰 (BYOK)」的安全優點。 

請完成下列步驟，以新增現有金鑰。

1. 在應用程式詳細資料頁面上，按一下**管理** &gt; **新增金鑰**。
2. 若要上傳現有金鑰，請選取**匯入自己的金鑰**視窗。

    指定金鑰的詳細資料：

    <table>
      <tr>
        <th>設定</th>
        <th>說明</th>
      </tr>
      <tr>
        <td>名稱</td>
        <td>
          <p>方便識別金鑰且人類可閱讀的唯一別名。</p>
          <p>若要保護您的隱私權，請確定金鑰名稱未包含個人識別資訊 (PII)（例如您的姓名或位置）。</p>
        </td>
      </tr>
      <tr>
        <td>金鑰類型</td>
        <td>您要在 {{site.data.keyword.keymanagementserviceshort}} 中管理的<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">金鑰類型</a>。</td>
      </tr>
      <tr>
        <td>金鑰資料</td>
        <td>您要在 {{site.data.keyword.keymanagementserviceshort}} 服務中儲存的金鑰資料（例如對稱金鑰）。您提供的金鑰必須以 base64 編碼。</td>
      </tr>
      <caption style="caption-side:bottom;">表 2. <b>匯入自己的金鑰</b>設定的說明</caption>
    </table>

3. 當您填寫完金鑰的詳細資料時，請按一下**匯入金鑰**以便確認。 

從 {{site.data.keyword.keymanagementserviceshort}} 儀表板中，您可以檢查新金鑰的一般特徵。 

## 下一步為何？
{: #get-started-next-steps}

現在，您可以使用金鑰來編碼應用程式及服務。如果您新增了根金鑰給服務，建議您進一步瞭解如何使用根金鑰來保護將您的靜態資料加密的金鑰。請參閱[包裝金鑰](/docs/services/key-protect?topic=key-protect-wrap-keys)以便開始。

- 若要進一步瞭解如何使用根金鑰來管理和保護加密金鑰，請參閱[使用封套加密保護資料](/docs/services/key-protect?topic=key-protect-envelope-encryption)。
- 若要進一步瞭解如何整合 {{site.data.keyword.keymanagementserviceshort}} 服務與其他雲端資料解決方案，[請參閱整合文件](/docs/services/key-protect?topic=key-protect-integrate-services)。
- 若要進一步瞭解如何以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。
