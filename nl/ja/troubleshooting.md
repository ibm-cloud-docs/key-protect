---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} の使用に関する一般的な問題には、API との対話の際に正しいヘッダーまたは資格情報を提供することが含まれることがあります。 多くの場合、いくつかの簡単なステップを実行することで、これらの問題から復旧することが可能です。
{: shortdesc}

## 鍵の作成または削除ができない
{: #unable-to-create-keys}

{{site.data.keyword.keymanagementserviceshort}} ユーザー・インターフェースにアクセスしたときに、鍵の追加や削除のためのオプションが表示されません。

{{site.data.keyword.cloud_notm}} ダッシュボードで、{{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを選択します。
{: tsSymptoms}

鍵のリストは表示されますが、鍵の追加や削除のためのオプションが表示されません。 

{{site.data.keyword.keymanagementserviceshort}} アクションを実行するための正しい許可を持っていません。
{: tsCauses} 

該当するリソース・グループまたはサービス・インスタンスにおいて正しい役割を割り当てられているかどうか、管理者と共に検証してください。 役割について詳しくは、[役割と許可](/docs/services/key-protect?topic=key-protect-manage-access#roles)を参照してください。
{: tsResolve}

## ヘルプおよびサポートの利用
{: #getting-help}

{{site.data.keyword.keymanagementserviceshort}} の使用中に問題や質問が生じた場合は、{{site.data.keyword.cloud_notm}} を調べてください。または、フォーラムで情報を検索したり質問したりして役に立つ情報を得ることもできます。 サポート・チケットを開くこともできます。
{: shortdesc}

[「状況」ページ](https://{DomainName}/status?tags=platform,runtimes,services){: external} にアクセスして、{{site.data.keyword.cloud_notm}} が使用可能かどうかを確認できます。

フォーラムを参照して、他のユーザーも同じ問題を経験していないか調べることができます。 フォーラムを使用して質問するときは、{{site.data.keyword.cloud_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

- {{site.data.keyword.keymanagementserviceshort}} について技術的な質問がある場合は、[Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} に質問を投稿し、その質問に「ibm-cloud」と「key-protect」のタグを付けてください。
- サービスや開始手順に関する質問については、[IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external} フォーラムを使用してください。「ibm-cloud」と「key-protect」のタグを付けてください。

フォーラムの使用について詳しくは、[サポートの利用](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external}を参照してください。

{{site.data.keyword.IBM_notm}} サポート・チケットのオープンや、サポート・レベルおよびチケットの重大度について詳しくは、[サポートへの問い合わせ](/docs/get-support?topic=get-support-getting-customer-support){: external}を参照してください。
