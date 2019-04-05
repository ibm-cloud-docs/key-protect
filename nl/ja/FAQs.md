---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

{{site.data.keyword.keymanagementservicelong}} の使用に役立つ FAQ を以下に記載します。

## {{site.data.keyword.keymanagementserviceshort}} の料金の仕組みはどのようなものですか
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} では、[段階的ティア・プラン](https://{DomainName}/catalog/services/key-protect)が用意されていて、必要な鍵が 20 個未満のユーザーでは無料の価格設定になっています。{{site.data.keyword.cloud_notm}} アカウント当たり 20 個の鍵を無料で使用できます。{{site.data.keyword.keymanagementserviceshort}} の複数インスタンスが必要なチームの場合、{{site.data.keyword.cloud_notm}} は、アクティブな鍵をアカウント内のすべてのインスタンスにわたって追加し、その後で価格設定を適用します。 

## アクティブな暗号鍵とは何ですか
{: #what-is-active-encryption-key}
{: faq}

暗号鍵を {{site.data.keyword.keymanagementserviceshort}} にインポートするか、または、{{site.data.keyword.keymanagementserviceshort}} を使用して HSM から鍵を生成すると、それらの鍵は_アクティブ_ な鍵になります。料金は、{{site.data.keyword.cloud_notm}} アカウント内のすべてのアクティブな鍵に基づきます。 

## 鍵をどのようにグループ化して管理すればよいでしょうか
{: #how-to-group-keys}
{: faq}

料金の観点からの {{site.data.keyword.keymanagementserviceshort}} の最良の使用方法は、限られた数のルート鍵を作成し、次に、それらのルート鍵を使用して、外部アプリまたはクラウド・データ・サービスによって作成されるデータ暗号鍵を暗号化することです。 

データ暗号鍵を保護するためのルート鍵の使用について詳しくは、[エンベロープ暗号化を使用したデータ保護](/docs/services/key-protect?topic=key-protect-envelope-encryption)を参照してください。

## ルート鍵とは何ですか
{: #what-is-root-key}
{: faq}

ルート鍵は、{{site.data.keyword.keymanagementserviceshort}} の 1 次リソースです。 ルート鍵は、データ・サービスに保管された他の鍵を[エンベロープ暗号化](/docs/services/key-protect?topic=key-protect-envelope-encryption)を使用して保護するための信頼の根源として使用される、対称鍵ラップ鍵です。{{site.data.keyword.keymanagementserviceshort}} を使用して、ルート鍵の作成、保管、およびライフサイクルの管理を行うことができます。これにより、クラウド内に保管された他の鍵を完全に制御できるようになります。 

## エンベロープ暗号化とは何ですか
{: #what-is-envelope-encryption}
{: faq}

エンベロープ暗号化とは、_データ暗号鍵_ を使用してデータを暗号化し、次にそのデータ暗号鍵を、高度にセキュアな_鍵ラップ鍵_ を使用して暗号化する手法です。保存データは複数レイヤーの暗号化を適用することによって保護されます。{{site.data.keyword.cloud_notm}} リソースに対してエンベロープ暗号化を有効にする方法について詳しくは、[サービスの統合](/docs/services/key-protect?topic=key-protect-integrate-services)を参照してください。

## 鍵の名前はどのくらいの長さにできますか。
{: #key-names}
{: faq}

最大 90 文字の長さの鍵の名前を使用できます。

## 鍵のメタデータとして個人情報を保管できますか
{: #personal-data}
{: faq}

個人データの機密性を保護するため、鍵のメタデータとして個人情報 (PII) を保管しないでください。個人情報には、自分の名前、住所、電話番号、E メール・アドレス、および自分自身や顧客、その他の人を特定したり、連絡を取ったり、居場所を見つけたりすることができるその他の情報が含まれます。


{{site.data.keyword.keymanagementserviceshort}} リソースおよび暗号鍵のメタデータとして保管する情報のセキュリティーを確保する責任はお客様にあります。その他の個人データの例については、[NIST Special Publication 800-122 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window} のセクション 2.2 を参照してください。
{: important}

## ある地域で作成された鍵を別の地域で使用できますか
{: #keys-across-regions}
{: faq}

暗号鍵は、それを作成した地域に封じ込まれています。 {{site.data.keyword.keymanagementserviceshort}} によって暗号鍵が他の地域にコピーされたりエクスポートされたりすることはありません。

## 鍵へのアクセス権限を誰が持つのかをどのように制御するのですか
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} では、暗号鍵のユーザーおよびアクセス権限の管理に役立つように、{{site.data.keyword.iamlong}} によって管理される集中アクセス制御システムをサポートしています。 サービスのセキュリティー管理者は、チームのメンバーに付与する[特定の {{site.data.keyword.keymanagementserviceshort}} 許可に対応する Cloud IAM 役割](/docs/services/key-protect?topic=key-protect-manage-access#roles)を割り当てることができます。

## {{site.data.keyword.keymanagementserviceshort}} への API 呼び出しをどのようにモニターするのですか
{: faq}

{{site.data.keyword.cloudaccesstrailfull_notm}} サービスを使用して、ユーザーおよびアプリケーションが {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスとどのように対話するのかを追跡できます。例えば、{{site.data.keyword.keymanagementserviceshort}} で鍵の作成、インポート、削除、または読み取りを行うと、{{site.data.keyword.cloudaccesstrailshort}} イベントが生成されます。 これらのイベントは、{{site.data.keyword.keymanagementserviceshort}} サービスがプロビジョンされる地域と同じ地域の {{site.data.keyword.cloudaccesstrailshort}} サービスに自動的に転送されます。

詳しくは、[Activity Tracker イベント](/docs/services/key-protect?topic=key-protect-activity-tracker-events)を参照してください。

## 鍵を削除するとどうなりますか。
{: #key-destruction}
{: faq}

鍵を削除すると、その鍵はサービスによって削除済みとマークされ、_破棄_ 状態に遷移します。この状態の鍵はもう復旧することはできず、その鍵を使用するクラウド・サービスは、その鍵と関連付けられたデータを暗号化解除できなくなります。データは暗号化された形式でそれらのサービスに残ります。鍵の遷移履歴や名前など、鍵に関連付けられているメタデータは、{{site.data.keyword.keymanagementserviceshort}} データベースに保管されます。 

鍵を削除する前に、その鍵と関連付けられたデータにはもうアクセスする必要がないことを確認してください。 このアクションは、元に戻すことはできません。

## サービス・インスタンスをプロビジョン解除する必要があるとどうなりますか
{: #deprovision-service}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} から移動すると決めた場合、残っている鍵をサービス・インスタンスから削除してからでないと、サービスをプロビジョン解除することはできません。サービス・インスタンスが削除された後、{{site.data.keyword.keymanagementserviceshort}} は、[エンベロープ暗号化](/docs/services/key-protect?topic=key-protect-envelope-encryption)を使用して、そのサービス・インスタンスと関連付けられたすべてのデータの暗号シュレッディングを行います。 
