---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

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

# ユーザーのアクセス権限の管理
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} では、暗号鍵のユーザーおよびアクセス権限の管理に役立つように、{{site.data.keyword.iamlong}} によって管理される集中アクセス制御システムをサポートしています。
{: shortdesc}

新規ユーザーをアカウントまたはサービスに招待する際にアクセス許可を付与することをお勧めします。 例えば、以下のガイドラインについて考慮してください。

- **Cloud IAM 役割を割り当てることで、アカウントのリソースに対するユーザーのアクセス権限を有効にします。**
    管理者の資格情報を共有するのではなく、管理者アカウントの暗号鍵にアクセスする必要があるユーザー向けに新規ポリシーを作成します。 アカウントの管理者には、アカウント内のすべてのリソースに対してアクセス権限を持つ_管理者_ ポリシーが自動的に割り当てられます。
- **必要最小限の有効範囲で、役割および許可を付与します。**
    例えば、ユーザーが指定のスペース内の鍵の概略ビューにのみアクセスする必要がある場合、そのユーザーには、そのスペースに対する_リーダー_ の役割のみを付与します。
- **アクセス制御の管理および鍵リソースの削除を行うことのできるユーザーを定期的に監査します。**
    ユーザーに対して_管理者_ の役割を付与することは、そのユーザーは他のユーザーのサービス・ポリシーを変更できるだけでなく、リソースを破棄することもできることを意味します。

## 役割と許可
{: #roles}

{{site.data.keyword.iamshort}} (IAM) を使用して、アカウント内のユーザーやリソースに対するアクセス権限を管理および定義できます。

アクセスを簡素化するために、{{site.data.keyword.keymanagementserviceshort}} は Cloud IAM 役割と調整し、ユーザーが割り当てられている役割に応じて、各ユーザーが異なるビューのサービスを持つようにします。 サービスのセキュリティー管理者は、チームのメンバーに付与する特定の {{site.data.keyword.keymanagementserviceshort}} 許可に対応する Cloud IAM 役割を割り当てることができます。

次の表は、ID とアクセス役割が {{site.data.keyword.keymanagementserviceshort}} 許可にどのようにマップされるかを示しています。

<table>
  <col width="20%">
  <col width="40%">
  <col width="40%">
  <tr>
    <th>サービスのアクセス役割</th>
    <th>説明</th>
    <th>アクション</th>
  </tr>
  <tr>
    <td><p>リーダー</p></td>
    <td><p>リーダーは、鍵の概略ビューを表示し、ラップおよびアンラップのアクションを実行できます。 リーダーは、鍵の素材へのアクセスおよび変更はできません。</p></td>
    <td>
      <p>
        <ul>
          <li>鍵の表示</li>
          <li>鍵のラップ</li>
          <li>鍵のアンラップ</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>ライター</p></td>
    <td><p>ライターは、鍵の作成、鍵の変更、鍵のローテート、および鍵素材へのアクセスを行えます。</p></td>
    <td>
      <p>
        <ul>
          <li>鍵の作成</li>
          <li>鍵の表示</li>
          <li>鍵のローテート</li>
          <li>鍵のラップ</li>
          <li>鍵のアンラップ</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>管理者</p></td>
    <td><p>管理者は、リーダーおよびライターが実行できるすべてのアクションを実行でき、鍵のローテーション・ポリシーの設定、鍵の削除、新規ユーザーの招待、および他のユーザーに対するアクセス・ポリシーの割り当ても実行できます。</p></td>
    <td>
      <p>
        <ul>
          <li>リーダーまたはライターが実行できるすべてのアクション</li>
          <li>ユーザー・アクセス・ポリシーの割り当て</li>
          <li>鍵のローテーション・ポリシーの設定</li>
          <li>鍵の削除</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">表 1. ID およびアクセス役割が {{site.data.keyword.keymanagementserviceshort}} 許可にどのようにマップされるのかについての説明</caption>
</table>

Cloud IAM ユーザー役割は、サービス・レベルまたはサービス・インスタンス・レベルでアクセス権限を提供します。 [Cloud Foundry 役割 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/iam?topic=iam-cfaccess){: new_window} は、これとは別個のもので、組織レベルまたはスペース・レベルでアクセス権限を定義します。 {{site.data.keyword.iamshort}} について詳しくは、[ユーザーの役割と許可 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/iam?topic=iam-userroles){: new_window} を参照してください。
{: note}

## 次に行うこと
{: #manage-access-next-steps}

アカウントの所有者および管理者は、ユーザーを招待し、ユーザーが実行できる {{site.data.keyword.keymanagementserviceshort}} アクションに対応するサービス・ポリシーを設定できます。

- {{site.data.keyword.cloud_notm}} UI でのユーザー役割の割り当てについて詳しくは、[IAM アクセス権限の管理 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/iam?topic=iam-getstarted){: new_window} を参照してください。

