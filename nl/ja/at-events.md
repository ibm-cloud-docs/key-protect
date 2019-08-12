---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Activity Tracker イベント
{: #at-events}

セキュリティー担当者、監査員、または管理者は、Activity Tracker サービスを使用して、ユーザーおよびアプリケーションが {{site.data.keyword.keymanagementservicefull}} とどのように対話するのかを追跡できます。
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker は、ユーザー開始アクティビティーを記録します。ユーザー開始アクティビティーにより、{{site.data.keyword.cloud_notm}} 内のサービスの状態は遷移します。このサービスを使用して、異常なアクティビティーや重大なアクションがないかを調査し、規制監査要件を遵守することができます。 さらに、アクションが発生した際にそれに関するアラートが通知されるようにすることもできます。 収集されるイベントは、Cloud Auditing Data Federation (CADF) 標準に準拠しています。 

現在、{{site.data.keyword.cloud_notm}} カタログには使用可能な 2 つの Activity Tracker サービスがあります。{{site.data.keyword.keymanagementserviceshort}} は両方のサービスにイベントを送信するため、いずれのサービスを使用しても {{site.data.keyword.cloud_notm}} での {{site.data.keyword.keymanagementserviceshort}} アクティビティーをモニターすることができます。 ただし、{{site.data.keyword.cloudaccesstrailfull}} は非推奨であるため、新規インスタンスを作成することはできません。また、既存のサービス・インスタンスのサポートを利用できるのは、2019 年 9 月 30 日までです。

詳しくは、以下を参照してください。
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (非推奨)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## イベントのリスト
{: #at-actions}

以下の表に、イベントを生成する {{site.data.keyword.keymanagementserviceshort}} アクションをリストします。

| アクション                   | 説明                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | 鍵を作成します                |
| `kms.secrets.read`       | ID によって鍵を取得します        |
| `kms.secrets.delete`     | ID によって鍵を削除します          |
| `kms.secrets.list`       | 鍵のリストを取得します     |
| `kms.secrets.head`       | 鍵の数を取得します |
| `kms.secrets.wrap`       | 鍵をラップします                  |
| `kms.secrets.unwrap`     | 鍵をアンラップします                |
| `kms.policies.read`      | 鍵のポリシーを表示します     |
| `kms.policies.write`     | 鍵のポリシーを設定します      |
{: caption="表 1. Activity Tracker イベントを生成する {{site.data.keyword.keymanagementserviceshort}} アクション" caption-side="top"}

## イベントの表示
{: #at-ui}

[{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) または [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (非推奨) を使用して、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに関連付けられている Activity Tracker イベントを表示することができます。

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### {{site.data.keyword.at_full_notm}} の使用
{: #at-ui-logdna}

{{site.data.keyword.keymanagementserviceshort}} のインスタンスによって生成されたイベントは、同じ場所で使用可能な {{site.data.keyword.at_full_notm}} サービス・インスタンスに自動的に転送されます。 

{{site.data.keyword.at_full_notm}} では、場所ごとに存在できるインスタンスは 1 つだけです。 イベントを表示するには、サービス・インスタンスが使用可能になっている場所と同じ場所で、{{site.data.keyword.at_full_notm}} サービスの Web UI にアクセスする必要があります。 詳しくは、[IBM Cloud UI を介して Web UI を起動する](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2)内容に関するセクションを参照してください。

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### {{site.data.keyword.cloudaccesstrailfull_notm}} の使用 (非推奨)
{: #at-ui-legacy}

{{site.data.keyword.cloudaccesstrailshort}} イベントは、イベントが生成される {{site.data.keyword.cloud_notm}} 地域で使用可能な {{site.data.keyword.cloudaccesstrailshort}} **アカウント・ドメイン**で使用できます。詳しくは、[イベントの表示](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4)を参照してください。


## イベントの分析
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

暗号鍵に関する情報の機密性により、{{site.data.keyword.keymanagementserviceshort}} サービスに対する API 呼び出しの結果でイベントが生成されるときに、生成されるイベントには鍵に関する詳細情報は含められません。 このイベントには、クラウド環境で内部的に鍵を識別するために使用できる相関 ID が含められます。 この相関 ID は、`responseBody.content` フィールドの一部として返されるフィールドです。 この情報を使用して、{{site.data.keyword.keymanagementserviceshort}} 鍵を、{{site.data.keyword.cloudaccesstrailshort}} イベントを通じて報告されたアクションの情報と相互に関連付けることができます。
