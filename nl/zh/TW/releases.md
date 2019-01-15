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

# 新增功能
{: #releases}

掌握最新的 {{site.data.keyword.keymanagementservicefull}} 可用新增特性。 

## 2018 年 10 月
{: #oct-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到亞太地區北部地區
新增日期：2018-10-31

您現在可以在亞太地區北部地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect/regions.html)。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式
新增日期：2018-10-02

您現在可以使用 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式，以在 {{site.data.keyword.keymanagementserviceshort}} 服務實例中管理金鑰。

若要了解如何安裝外掛程式，請參閱[設定 CLI](/docs/services/key-protect/set-up-cli.html)。若要進一步了解 {{site.data.keyword.keymanagementserviceshort}} CLI，[請參閱 CLI 參考資料文件](/docs/services/key-protect/cli-reference.html)。

## 2018 年 9 月
{: #sept-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了對隨需應變金鑰替換的支援
新增日期：2018-09-28

您現在可以使用 {{site.data.keyword.keymanagementserviceshort}}，隨需應變地替換您的主要金鑰。

如需相關資訊，請參閱[替換金鑰](/docs/services/key-protect/rotate-keys.html)。

### 已新增：雲端應用程式的端對端安全指導教學
新增日期：2018-09-14

在尋找程式碼範例以協助您用自己的加密金鑰將儲存空間儲存區內容加密？

您現在可以遵循[新的指導教學](/docs/tutorials/cloud-e2e-security.html)，練習為雲端應用程式新增端對端安全。

如需相關資訊，[請參閱 GitHub 中的範例應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到美國東部地區
新增日期：2018-09-10

您現在可以在美國東部地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect/regions.html)。

## 2018 年 8 月
{: #aug-2018}

### 已變更：{{site.data.keyword.keymanagementserviceshort}} API 文件 URL
新增日期：2018-08-28

{{site.data.keyword.keymanagementserviceshort}} API 參考資料已移動！ 

您現在可以在下列網址存取 API 文件：
https://console.bluemix.net/apidocs/key-protect. 

## 2018 年 3 月
{: #mar-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到法蘭克福地區
新增日期：2018-03-21

您現在可以在法蘭克福地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect/regions.html)。

## 2018 年 1 月
{: #jan-2018}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到雪梨地區
新增日期：2018-01-31

您現在可以在雪梨地區建立 {{site.data.keyword.keymanagementserviceshort}} 資源。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect/regions.html)。

## 2017 年 12 月
{: #dec-2017}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增對「自攜金鑰 (BYOK)」的支援
新增日期：2017-12-15

{{site.data.keyword.keymanagementserviceshort}} 現在支援「自攜金鑰 (BYOK)」及客戶管理的加密

- 已引進[主要金鑰](/docs/services/key-protect/concepts/envelope-encryption.html#key-types)，也稱為「客戶主要金鑰 (CRK)」，作為服務中的主要資源。 
- 已啟用 {{site.data.keyword.cos_full_notm}} 儲存區的[封套加密](/docs/services/key-protect/integrations/integrate-cos.html#kp-cos-how)。

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 擴展到倫敦地區
新增日期：2017-12-15

現在可以在倫敦地區使用 {{site.data.keyword.keymanagementserviceshort}}。 

如需相關資訊，請參閱[地區及位置](/docs/services/key-protect/regions.html)。

### 已變更：{{site.data.keyword.iamshort}} 角色
新增日期：2017-12-15

{{site.data.keyword.iamshort}} 角色已變更，這些角色決定您可以對 {{site.data.keyword.keymanagementserviceshort}} 資源執行的動作。

- `Administrator` 現在是 `Manager`
- `Editor` 現在是 `Writer`
- `Viewer` 現在是 `Reader`

如需相關資訊，請參閱[管理使用者存取](/docs/services/key-protect/manage-access.html)。

## 2017 年 9 月
{: #sept-2017}

### 已新增：{{site.data.keyword.keymanagementserviceshort}} 新增了對 Cloud IAM 的支援
新增日期：2017-09-19

您現在可以使用 {{site.data.keyword.iamshort}} 來設定和管理 {{site.data.keyword.keymanagementserviceshort}} 資源的存取原則。

如需相關資訊，請參閱[管理使用者存取](/docs/services/key-protect/manage-access.html)。
