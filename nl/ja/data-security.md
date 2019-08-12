---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# セキュリティーとコンプライアンス
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} には、ユーザーのコンプライアンス要件を満たし、データをクラウド内で確実にセキュアに保護するために、データ・セキュリティー戦略があります。
{: shortdesc}

## セキュリティー対応
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} は、システム、ネットワーキング、およびセキュアなエンジニアリングに関する {{site.data.keyword.IBM_notm}} のベスト・プラクティスに従うことで、セキュリティー対応を確実なものにします。 

{{site.data.keyword.cloud_notm}} 全体のセキュリティー管理について詳しくは、[データの安全性を確認する方法](/docs/overview?topic=overview-security#security)を参照してください。
{: tip}

### データ暗号化
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} は、[{{site.data.keyword.cloud_notm}} ハードウェア・セキュリティー・モジュール (HSM)](https://www.ibm.com/cloud/hardware-security-module){: external} を使用して、プロバイダー管理の鍵素材を生成し、[エンベロープ暗号化](/docs/services/key-protect?topic=key-protect-envelope-encryption)操作を実行します。 HSM は、改ざんに耐性のあるハードウェア・デバイスであり、暗号境界外部に鍵を公開することなく暗号鍵素材を保管および使用します。

このサービスへのアクセスは HTTPS を介して実行され、内部 {{site.data.keyword.keymanagementserviceshort}} 通信では Transport Layer Security (TLS) 1.2 プロトコルを使用して転送中データが暗号化されます。

### データ削除
{: #data-deletion}

鍵を削除すると、その鍵はサービスによって削除済みとマークされ、_破棄_ 状態に遷移します。 この状態の鍵はもう復旧することはできず、その鍵を使用するクラウド・サービスは、その鍵と関連付けられたデータを暗号化解除できなくなります。 データは暗号化された形式でそれらのサービスに残ります。 鍵の遷移履歴や名前など、鍵に関連付けられているメタデータは、{{site.data.keyword.keymanagementserviceshort}} データベースに保管されます。 

{{site.data.keyword.keymanagementserviceshort}} での鍵の削除は、破壊的な操作です。 鍵の削除後はその操作を元に戻すことはできず、その鍵と関連付けられたデータは鍵が削除された時点で即時に失われることに留意してください。 鍵を削除する前に、その鍵と関連付けられたデータを検討し、もうアクセスする必要がないことを確認してください。 実稼働環境でデータをアクティブに保護している鍵を削除しないでください。 

鍵で保護されているデータを判別するのに役立てるため、`ibmcloud iam authorization-policies` を実行することによって、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスがどのように既存のクラウド・サービスにマップされているのかを確認できます。 コンソールでのサービス許可の表示について詳しくは、[サービス間のアクセスの認可](/docs/iam?topic=iam-serviceauth)を参照してください。
{: note}

## コンプライアンス対応
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} はグローバル、業界、および地域のコンプライアンス規格 (GDPR、HIPAA、および ISO 27001/27017/27018 など) の規制に準拠しています。 

{{site.data.keyword.cloud_notm}} のコンプライアンス認証の完全なリストについては、[{{site.data.keyword.cloud_notm}} のコンプライアンス対応](https://www.ibm.com/cloud/compliance){: external}を参照してください。
{: tip}

### EU サポート
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} には、EU 内の {{site.data.keyword.keymanagementserviceshort}} リソースを保護するための追加の制御機能が用意されています。 

ドイツ地域のフランクフルトにある {{site.data.keyword.keymanagementserviceshort}} リソースを使用して欧州市民の個人データを処理する場合などは、{{site.data.keyword.cloud_notm}} アカウントで「EU サポート対象」設定を有効にすることができます。 詳しくは、 [「EU サポート対象」設定の有効化](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)および [EU 内のリソースに関するサポートの依頼](/docs/get-support?topic=get-support-getting-customer-support#eusupported)を参照してください。

### 一般データ保護規則 (GDPR)
{: #gdpr}

GDPR は、EU 全域における統一されたデータ保護の法的枠組みを作成しようというものであり、個人データの管理権限を市民の手に取り戻すことを目的とし、世界のどこであろうと個人データをホスティングおよび処理するものに対して厳格な規則を課します。

{{site.data.keyword.IBM_notm}} は、お客様および {{site.data.keyword.IBM_notm}} ビジネス・パートナーに、データ・プライバシー、セキュリティー、およびガバナンスに関する革新的なソリューションを提供することに努め、GDPR 対応までの過程を支援します。 

{{site.data.keyword.keymanagementserviceshort}} リソースを確実に GDPR に準拠させるには、{{site.data.keyword.cloud_notm}} アカウントで [「EU サポート対象」設定の有効化](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)を行います。 {{site.data.keyword.keymanagementserviceshort}} による個人データの処理および保護の方法について詳しくは、以下の補足契約書を確認してください。

- [{{site.data.keyword.keymanagementservicefull_notm}} データ・シート補足契約書 (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} データ処理補足契約書 (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA サポート
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} は、米国における医療保険の積算と責任に関する法律 (HIPAA) の規制に準拠しており、保護医療情報 (PHI) を確実に保護します。 

お客様またはお客様の会社が、HIPAA が定める適用対象事業者または事業体である場合は、{{site.data.keyword.cloud_notm}} アカウントで「HIPAA サポートあり」設定を有効にすることができます。 詳しくは、[「HIPAA サポートあり」設定の有効化](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa)を参照してください。

### ISO 27001、27017、27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} は、ISO 27001、27017、27018 の各認証を取得しています。 [{{site.data.keyword.cloud_notm}} のコンプライアンス対応](https://www.ibm.com/cloud/compliance){: external}にアクセスすると、コンプライアンス認証を確認できます。 

### SOC 2 タイプ 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} は、SOC 2 タイプ 1 の認証を取得しています。 {{site.data.keyword.cloud_notm}} SOC 2 レポートの要求について詳しくは、[{{site.data.keyword.cloud_notm}} のコンプライアンス対応](https://www.ibm.com/cloud/compliance){: external}を参照してください。
