---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# セキュリティーとコンプライアンス
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} には、ユーザーのコンプライアンス要件を満たし、データをクラウド内で確実に保護するために、データ・セキュリティー戦略があります。
{: shortdesc}

## データ・セキュリティーと暗号化
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} は、[{{site.data.keyword.cloud_notm}} ハードウェア・セキュリティー・モジュール (HSM) ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/hardware-security-module) を使用して、プロバイダー管理の鍵素材を生成し、[エンベロープ暗号化](/docs/services/key-protect?topic=key-protect-envelope-encryption)操作を実行します。 HSM は、改ざんに耐性のあるハードウェア・デバイスであり、暗号境界外部に鍵を公開することなく暗号鍵素材を保管および使用します。

このサービスへのアクセスは HTTPS を介して実行され、内部 {{site.data.keyword.keymanagementserviceshort}} 通信では Transport Layer Security (TLS) 1.2 プロトコルを使用して転送中データが暗号化されます。

{{site.data.keyword.keymanagementserviceshort}} がどのようにユーザーのコンプライアンス要件を満たすのかについて詳しくは、[プラットフォームおよびサービスのコンプライアンス](/docs/overview?topic=overview-security#compliancetable)を参照してください。

## データ削除
{: #data-deletion}

鍵を削除すると、その鍵はサービスによって削除済みとマークされ、_破棄_ 状態に遷移します。 この状態の鍵はもう復旧することはできず、その鍵を使用するクラウド・サービスは、その鍵と関連付けられたデータを暗号化解除できなくなります。 データは暗号化された形式でそれらのサービスに残ります。 鍵の遷移履歴や名前など、鍵に関連付けられているメタデータは、{{site.data.keyword.keymanagementserviceshort}} データベースに保管されます。 

{{site.data.keyword.keymanagementserviceshort}} での鍵の削除は、破壊的な操作です。 鍵の削除後はその操作を元に戻すことはできず、その鍵と関連付けられたデータは鍵が削除された時点で即時に失われることに留意してください。 鍵を削除する前に、その鍵と関連付けられたデータを検討し、もうアクセスする必要がないことを確認してください。 実稼働環境でデータをアクティブに保護している鍵を削除しないでください。 

鍵で保護されているデータを判別するのに役立てるため、`ibmcloud iam authorization-policies` を実行することによって、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスがどのように既存のクラウド・サービスにマップするのかを確認できます。 コンソールでのサービス許可の表示について詳しくは、[サービス間のアクセスの認可](/docs/iam?topic=iam-serviceauth)を参照してください。
{: note}
