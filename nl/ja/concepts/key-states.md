---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: encryption key states, encryption key lifecycle, manage key lifecycle

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

# 暗号鍵のライフサイクルのモニター
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} は、[NIST SP 800-57 for key states](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external} によるセキュリティー・ガイドラインに従っています。
{: shortdesc}

## 鍵の状態および遷移
{: #key-transitions}

暗号鍵は、存続期間中にいくつかの状態を遷移していきます。これらの状態は、鍵が存続している期間の長さ、およびデータが保護されているかどうかに応じて変化します。 

{{site.data.keyword.keymanagementserviceshort}} で提供されているグラフィカル・ユーザー・インターフェースまたは REST API を使用して、鍵のライフサイクルにおける状態の変化を追跡できます。 次の図は、鍵がその生成から消滅までの間にいくつかの状態を経過する様子を示しています。

![図には、以下の定義表に説明されているのと同じコンポーネントが表示されています。](../images/key-states_min.svg)

| 状態 | 説明 |
| --- | --- |
| アクティベーション前 | 鍵は、最初に_アクティベーション前_ 状態で作成されます。 データを暗号で保護するためにアクティブ化前の鍵を使用することはできません。|
| アクティブ | 鍵は、アクティベーション日に即時に_アクティブ_ 状態に遷移します。 この遷移は、鍵の暗号期間の開始をマークします。 アクティベーション日が指定されていない鍵は、即時にアクティブになり、有効期限が切れるか破棄されるまでアクティブのままになります。 |
| 非アクティブ化 | 有効期限が割り当てられている場合、鍵は有効期限日に_非アクティブ化_ 状態に遷移します。 この状態では、鍵は暗号化によってデータを保護することはできず、_破棄_ 状態にのみ遷移できます。|
| 破棄 | 削除された鍵は、_破棄_ 状態になります。 この状態の鍵は、リカバリーできません。 鍵の遷移履歴や名前など、鍵に関連付けられているメタデータは、{{site.data.keyword.keymanagementserviceshort}} データベースに保管されます。 |
{: caption="表 1. 鍵の状態および遷移についての説明" caption-side="top"}

サービスに鍵を追加した後、{{site.data.keyword.keymanagementserviceshort}} ダッシュボードまたは {{site.data.keyword.keymanagementserviceshort}} REST API を使用して、鍵の遷移の履歴および構成を確認できます。 監査の目的では、{{site.data.keyword.keymanagementserviceshort}} を {{site.data.keyword.cloudaccesstrailfull}} と統合することで、鍵のアクティビティー証跡をモニターすることもできます。 両方のサービスがプロビジョンされて実行された後、{{site.data.keyword.keymanagementserviceshort}} で鍵を作成したり削除したりすると、アクティビティー・イベントが生成され、自動的に {{site.data.keyword.cloudaccesstrailshort}} ログに収集されます。 

詳しくは、[{{site.data.keyword.keymanagementserviceshort}} アクティビティーのモニター](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: external}を参照してください。
