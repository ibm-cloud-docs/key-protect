---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: user permissions, manage access, IAM roles

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

# 管理使用者存取
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} 支援由 {{site.data.keyword.iamlong}} 控管的集中式存取控制系統，可協助您管理使用者和加密金鑰的存取。
{: shortdesc}

有一項良好的作法，就是在您邀請新使用者加入帳戶或服務時，授與存取權。例如，請考量下列準則：

- **藉由指派 Cloud IAM 角色，讓使用者可存取您帳戶中的資源。**
    為需要存取您帳戶中之加密金鑰的使用者建立新原則，而不是共用您的管理認證。如果您是帳戶的管理者，您會自動獲指派有權存取帳戶下所有資源的_管理員_ 原則。
- **以最小的需要範圍授與角色及許可權。**
    例如，如果使用者只需要存取指定空間內的金鑰高階視圖，請將_讀者_ 角色授與該空間的使用者。
- **定期審核可管理存取控制以及刪除金鑰資源的人。**
    請記住，將_管理員_ 角色授與使用者，表示該使用者除了可以破壞資源之外，還能修改其他使用者的服務原則。

## 角色及許可權
{: #roles}

使用 {{site.data.keyword.iamshort}} (IAM)，您可以管理及定義帳戶中使用者和資源的存取權。

為了簡化存取，{{site.data.keyword.keymanagementserviceshort}} 會與 Cloud IAM 角色一致，讓每個使用者根據使用者獲得指派的角色，而有不同的服務視圖。如果您是服務的安全管理者，則可以指派對應至您想要授與給團隊成員之特定 {{site.data.keyword.keymanagementserviceshort}} 許可權的 Cloud IAM 角色。

下表顯示身分及存取角色如何對映至 {{site.data.keyword.keymanagementserviceshort}} 許可權：

<table>
  <col width="20%">
  <col width="40%">
  <col width="40%">
  <tr>
    <th>服務存取角色</th>
    <th>說明</th>
    <th>動作</th>
  </tr>
  <tr>
    <td><p>讀者</p></td>
    <td><p>讀者可以瀏覽高階金鑰視圖，並執行 wrap 及 unwrap 動作。讀者無法存取或修改金鑰資料。</p></td>
    <td>
      <p>
        <ul>
          <li>檢視金鑰</li>
          <li>包裝金鑰</li>
          <li>解除包裝金鑰</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>作者</p></td>
    <td><p>作者可以建立金鑰、修改金鑰、替換金鑰，以及存取金鑰資料。</p></td>
    <td>
      <p>
        <ul>
          <li>建立金鑰</li>
          <li>檢視金鑰</li>
          <li>替換金鑰</li>
          <li>包裝金鑰</li>
          <li>解除包裝金鑰</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>管理員</p></td>
    <td><p>管理員可以執行讀者和作者可執行的所有動作，包括能夠設定金鑰的替換原則、刪除金鑰、邀請新使用者，以及指派其他使用者的存取原則。</p></td>
    <td>
      <p>
        <ul>
          <li>讀者或作者可執行的所有動作</li>
          <li>指派使用者存取原則</li>
          <li>設定金鑰替換原則</li>
          <li>刪除金鑰</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 說明身分及存取角色如何對映至 {{site.data.keyword.keymanagementserviceshort}} 許可權</caption>
</table>

Cloud IAM 使用者角色提供服務或服務實例層次的存取權。[Cloud Foundry 角色 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/iam?topic=iam-cfaccess){: new_window} 是分開的，並且定義組織或空間層次的存取權。若要進一步瞭解 {{site.data.keyword.iamshort}}，請參閱[使用者角色及許可權 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/iam?topic=iam-userroles){: new_window}。
{: note}

## 下一步為何？
{: #manage-access-next-steps}

帳戶擁有者及管理者可以邀請使用者，以及設定對應於使用者可執行之 {{site.data.keyword.keymanagementserviceshort}} 動作的服務原則。

- 如需在 {{site.data.keyword.cloud_notm}} 使用者介面中指派使用者角色的相關資訊，請參閱[管理 IAM 存取權 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/iam?topic=iam-getstarted){: new_window}。

