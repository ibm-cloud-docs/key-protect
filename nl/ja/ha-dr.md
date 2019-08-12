---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect availability, Key Protect disaster recovery

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

# 高可用性と災害復旧
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} は、高度に使用可能な地域サービスで、アプリケーションを保護して作動させるための自動機能を備えています。
{: shortdesc}

このページでは、可用性と災害復旧に関する {{site.data.keyword.keymanagementserviceshort}} の戦略について説明します。

## 場所、テナント、可用性
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} はマルチテナントの地域サービスです。 

{{site.data.keyword.keymanagementserviceshort}} リソースは、サポートされているいずれかの [{{site.data.keyword.cloud_notm}} 地域](/docs/services/key-protect?topic=key-protect-regions#regions)に作成できます。これらの地域は、{{site.data.keyword.keymanagementserviceshort}} の要求が扱われて処理される地理上のエリアを表します。 {{site.data.keyword.cloud_notm}} の各地域には、その地域のローカル・アクセス権限、低遅延、セキュリティーに関する要件を満たすために[複数のアベイラビリティー・ゾーン](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external}が用意されています。

{{site.data.keyword.cloud_notm}} を使用した保存データの暗号化戦略を計画する際、最寄りの地域で {{site.data.keyword.keymanagementserviceshort}} をプロビジョニングすれば、{{site.data.keyword.keymanagementserviceshort}} API と対話作業をするときに接続の速度と信頼性が向上する可能性が高くなります。 {{site.data.keyword.keymanagementserviceshort}} リソースに依存するユーザー、アプリ、またはサービスが地理的に集中している場合は、特定の地域を選択します。 その地域から遠いユーザーとサービスは待ち時間が長くなる可能性があります。 

暗号鍵の使用は、それを作成した地域に限定されます。 {{site.data.keyword.keymanagementserviceshort}} によって暗号鍵が他の地域にコピーされたりエクスポートされたりすることはありません。
{: note}

## 災害復旧
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} の地域災害復旧では、目標復旧時間 (RTO) が 1 時間になっています。 このサービスは、災害時の計画と復旧に関して {{site.data.keyword.cloud_notm}} の要件に従います。 詳しくは、[災害復旧](/docs/overview?topic=overview-zero-downtime#disaster-recovery)を参照してください。


