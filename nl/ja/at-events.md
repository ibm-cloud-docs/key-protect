---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloudaccesstrailshort}} イベント
{: #activity-tracker-events}

{{site.data.keyword.cloudaccesstrailfull}} サービスを使用して、ユーザーとアプリケーションが {{site.data.keyword.keymanagementservicefull}} と対話する方法を追跡します。 
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} サービスは、{{site.data.keyword.cloud_notm}} のサービスの状態を変更する、ユーザーが開始したアクティビティーを記録します。 

詳しくは、[{{site.data.keyword.cloudaccesstrailshort}} の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla){: new_window} を参照してください。

## イベントのリスト
{: #list-activity-tracker-events}

以下の表に、イベントを生成するアクションをリストします。

| アクション               | 説明                 |
| -------------------- | --------------------------- |
| `kms.secrets.create` | 鍵を作成します                |
| `kms.secrets.read`   | ID によって鍵を取得します        |
| `kms.secrets.delete` | ID によって鍵を削除します          |
| `kms.secrets.list`   | 鍵のリストを取得します     |
| `kms.secrets.head`   | 鍵の数を取得します |
| `kms.secrets.wrap`   | 鍵をラップします                  |
| `kms.secrets.unwrap` | 鍵をアンラップします                |
| `kms.policies.read`  | 鍵のポリシーを表示します     |
| `kms.policies.write` | 鍵のポリシーを設定します      |
{: caption="表 1. {{site.data.keyword.cloudaccesstrailfull_notm}} イベントを生成するアクション" caption-side="top"}

## イベントの表示場所
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} イベントは、イベントが生成される {{site.data.keyword.cloud_notm}} 地域で使用可能な {{site.data.keyword.cloudaccesstrailshort}} **アカウント・ドメイン**で使用できます。

例えば、{{site.data.keyword.keymanagementserviceshort}} で鍵の作成、インポート、削除、または読み取りを行うと、{{site.data.keyword.cloudaccesstrailshort}} イベントが生成されます。 これらのイベントは、{{site.data.keyword.keymanagementserviceshort}} サービスがプロビジョンされる地域と同じ地域の {{site.data.keyword.cloudaccesstrailshort}} サービスに自動的に転送されます。

API アクティビティーをモニターするには、{{site.data.keyword.keymanagementserviceshort}} サービスがプロビジョンされる地域と同じ地域で使用可能なスペースで {{site.data.keyword.cloudaccesstrailshort}} サービスをプロビジョンする必要があります。 その後、ライト・プランを利用している場合は {{site.data.keyword.cloudaccesstrailshort}} UI のアカウント・ビューから、プレミアム・プランを利用している場合は Kibana からイベントを表示することができます。

## 追加情報
{: #activity-tracker-info}

暗号鍵に関する情報の機密性により、{{site.data.keyword.keymanagementserviceshort}} サービスに対する API 呼び出しの結果でイベントが生成されるときに、生成されるイベントには鍵に関する詳細情報は含められません。 このイベントには、クラウド環境で内部的に鍵を識別するために使用できる相関 ID が含められます。 この相関 ID は、`responseHeader.content` フィールドの一部として返されるフィールドです。 この情報を使用して、{{site.data.keyword.keymanagementserviceshort}} 鍵を、{{site.data.keyword.cloudaccesstrailshort}} イベントを通じて報告されたアクションの情報と相互に関連付けることができます。
