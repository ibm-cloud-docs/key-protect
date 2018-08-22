---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} の使用に関する一般的な問題には、API との対話の際に正しいヘッダーまたは資格情報を提供することが含まれることがあります。多くの場合、いくつかの簡単なステップを実行することで、これらの問題から復旧することが可能です。
{: shortdesc}

## ユーザー・インターフェースにアクセスできない
{: #unabletoaccessUI}

{{site.data.keyword.keymanagementserviceshort}} ユーザー・インターフェースにアクセスしたときに、予期したように UI がロードされません。

{{site.data.keyword.cloud_notm}} ダッシュボードで、{{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを選択します。
{: tsSymptoms}

以下のエラーが表示されます。 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

2017 年 12 月 15 日に、新しいフィーチャー ([エンベロープ暗号化](/docs/services/keymgmt/concepts/keyprotect_envelope.html)など) が {{site.data.keyword.keymanagementserviceshort}} サービスに追加されました。Cloud Foundry 組織およびスペースを指定する必要なく、{{site.data.keyword.keymanagementserviceshort}} サービスをグローバルにプロビジョンできるようになりました。
{: tsCauses}

これらの変更により、古いサービス・インスタンスのユーザー・インターフェースに影響がありました。{{site.data.keyword.keymanagementserviceshort}} のインスタンスを作成したのが 2017 年 9 月 28 日より前だった場合、ユーザー・インターフェースが予期されたようには動作しないことがあります。

この問題を修正するための作業が行われているところです。一時的な解決策として、{{site.data.keyword.keymanagementserviceshort}} API を使用して鍵の管理を続行できます。
{: tsResolve}

レガシー `https://ibm-key-protect.edge.bluemix.net` エンドポイントを使用して、{{site.data.keyword.keymanagementserviceshort}} サービスと対話できます。

**要求の例**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## 鍵の作成または削除ができない
{: #unabletomanagekeys}

{{site.data.keyword.keymanagementserviceshort}} ユーザー・インターフェースにアクセスしたときに、鍵の追加や削除のためのオプションが表示されません。

{{site.data.keyword.cloud_notm}} ダッシュボードで、{{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを選択します。
{: tsSymptoms}

鍵のリストは表示されますが、鍵の追加や削除のためのオプションが表示されません。 

{{site.data.keyword.keymanagementserviceshort}} アクションを実行するための正しい許可を持っていません。
{: tsCauses} 

該当するリソース・グループまたはサービス・インスタンスにおいて正しい役割を割り当てられているかどうか、管理者と共に検証してください。役割について詳しくは、[役割と許可](/docs/services/keymgmt/keyprotect_manage_access.html#roles)を参照してください。
{: tsResolve}

## ヘルプおよびサポートの利用
{: #getting_help}

{{site.data.keyword.keymanagementserviceshort}} の使用中に問題や質問が生じた場合は、{{site.data.keyword.cloud_notm}} を調べてください。または、フォーラムで情報を検索したり質問したりして役に立つ情報を得ることもできます。サポート・チケットを開くこともできます。
{: shortdesc}

[「状況」ページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/status?tags=platform,runtimes,services) にアクセスして、{{site.data.keyword.cloud_notm}} が使用可能かどうかを確認できます。

フォーラムを参照して、他のユーザーも同じ問題を経験していないか調べることができます。フォーラムを使用して質問するときは、{{site.data.keyword.cloud_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

- {{site.data.keyword.keymanagementserviceshort}} について技術的な質問がある場合は、[Stack Overflow ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} に質問を投稿し、その質問に「ibm-cloud」と「key-protect」のタグを付けてください。
- サービスや使用開始の手順についての質問には、[IBM developerWorks dW Answers ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} フォーラムを使用してください。 「ibm-cloud」と「key-protect」のタグを付けてください。

フォーラムの使用について詳しくは、[ヘルプの利用 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window} を参照してください。

{{site.data.keyword.IBM_notm}} サポート・チケットのオープンや、サポート・レベルおよびチケットの重大度について詳しくは、[サポートへの問い合わせ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window} を参照してください。
