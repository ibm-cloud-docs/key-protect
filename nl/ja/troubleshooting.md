---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

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
{:note: .note}
{:important: .important}

# トラブルシューティング
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} の使用に関する一般的な問題には、API との対話の際に正しいヘッダーまたは資格情報を提供することが含まれることがあります。 多くの場合、いくつかの簡単なステップを実行することで、これらの問題から復旧することが可能です。
{: shortdesc}

## Cloud Foundry サービス・インスタンスを削除できない
{: #unable-to-delete-service}

{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを削除しようとすると、サービスが期待どおりに削除されません。

{{site.data.keyword.cloud_notm}} ダッシュボードで**「Cloud Foundry サービス」**にナビゲートし、{{site.data.keyword.keymanagementserviceshort}} のインスタンスを選択します。「⋮」アイコンをクリックしてサービス・オファリングのオプションのリストを開き、**「サービスの削除」**をクリックします。
{: tsSymptoms}

サービスの削除に失敗し、以下のエラーが表示されます。 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

2017 年 12 月 15 日に、{{site.data.keyword.keymanagementserviceshort}} は、Cloud Foundry の組織、スペース、役割の使用から IAM およびリソース・グループの使用に移行しました。Cloud Foundry の組織およびスペースを指定することなく、リソース・グループ内で {{site.data.keyword.keymanagementserviceshort}} サービスをプロビジョンできるようになりました。
{: tsCauses}

こうした変更により、サービスの古いインスタンスでのプロビジョン解除の動作に影響がありました。{{site.data.keyword.keymanagementserviceshort}} のインスタンスを作成したのが 2017 年 9 月 28 日より前だった場合、サービスの削除は期待どおりに機能しないことがあります。

古い {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを削除するには、まず、レガシー `https://ibm-key-protect.edge.bluemix.net` エンドポイントを使用して {{site.data.keyword.keymanagementserviceshort}} サービスと対話することで、既存の鍵を削除する必要があります。
{: tsResolve}

鍵およびサービス・インスタンスを削除するには、以下のようにします。

1. {{site.data.keyword.cloud_notm}} CLI を使用して、{{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **注:** ログインに失敗した場合は、`bx login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。

2. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが含まれている {{site.data.keyword.cloud_notm}} の地域、組織、およびスペースを選択します。

    CLI 出力に表示される組織およびスペースの名前をメモしてください。 `ibmcloud cf target` を実行してご使用の Cloud Foundry の組織およびスペースをターゲットにすることもできます。

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. {{site.data.keyword.cloud_notm}} 組織およびスペース GUID を取得します。

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
    `<organization_name>` および `<space_name>` は、ご使用の組織およびスペースに割り当てた固有の別名に置き換えてください。

4. アクセス・トークンを取得します。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. 以下の cURL コマンドを実行して、サービス・インスタンスで保管されている鍵をリストします。

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`、`<organization_GUID>`、および `<space_GUID>` は、ステップ 3 と 4 で取得した値に置き換えてください。 

6. サービス・インスタンスで保管されている各鍵の ID 値をコピーします。

7. 次の cURL コマンドを実行して、鍵とその内容を完全に削除します。

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`、`<organization_GUID>`、`<space_GUID>`、および `<key_ID>` は、ステップ 3 から 5 で取得した値に置き換えてください。鍵ごとにコマンドを繰り返します。    

8. 以下の cURL コマンドを実行して、鍵が削除されたことを確認します。

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`、`<organization_GUID>`、および `<space_GUID>` は、ステップ 3 と 4 で取得した値に置き換えてください。 

9. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを削除します。

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. オプション: {{site.data.keyword.cloud_notm}} ダッシュボードにナビゲートして、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが削除されたことを確認します。

    以下のコマンドを実行することで、ターゲットのスペースで使用可能な Cloud Foundry サービスをリストすることもできます。

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## ユーザー・インターフェースにアクセスできない
{: #unable-to-access-ui}

{{site.data.keyword.keymanagementserviceshort}} ユーザー・インターフェースにアクセスしたときに、予期したように UI がロードされません。

{{site.data.keyword.cloud_notm}} ダッシュボードで、{{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを選択します。
{: tsSymptoms}

以下のエラーが表示されます。 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

2017 年 12 月 15 日に、新しいフィーチャー ([エンベロープ暗号化](/docs/services/key-protect/concepts/envelope-encryption.html)など) が {{site.data.keyword.keymanagementserviceshort}} サービスに追加されました。 Cloud Foundry の組織およびスペースを指定することなく、リソース・グループ内で {{site.data.keyword.keymanagementserviceshort}} サービスをプロビジョンできるようになりました。
{: tsCauses}

これらの変更により、古いサービス・インスタンスのユーザー・インターフェースに影響がありました。 {{site.data.keyword.keymanagementserviceshort}} のインスタンスを作成したのが 2017 年 9 月 28 日より前だった場合、ユーザー・インターフェースが予期されたようには動作しないことがあります。

この問題を修正するための作業が行われているところです。 一時的な解決策として、{{site.data.keyword.keymanagementserviceshort}} API を使用して鍵の管理を続行できます。
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
{: #unable-to-create-keys}

{{site.data.keyword.keymanagementserviceshort}} ユーザー・インターフェースにアクセスしたときに、鍵の追加や削除のためのオプションが表示されません。

{{site.data.keyword.cloud_notm}} ダッシュボードで、{{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを選択します。
{: tsSymptoms}

鍵のリストは表示されますが、鍵の追加や削除のためのオプションが表示されません。 

{{site.data.keyword.keymanagementserviceshort}} アクションを実行するための正しい許可を持っていません。
{: tsCauses} 

該当するリソース・グループまたはサービス・インスタンスにおいて正しい役割を割り当てられているかどうか、管理者と共に検証してください。 役割について詳しくは、[役割と許可](/docs/services/key-protect/manage-access.html#roles)を参照してください。
{: tsResolve}

## ヘルプおよびサポートの利用
{: #getting-help}

{{site.data.keyword.keymanagementserviceshort}} の使用中に問題や質問が生じた場合は、{{site.data.keyword.cloud_notm}} を調べてください。または、フォーラムで情報を検索したり質問したりして役に立つ情報を得ることもできます。 サポート・チケットを開くこともできます。
{: shortdesc}

[「状況」ページ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/status?tags=platform,runtimes,services) にアクセスして、{{site.data.keyword.cloud_notm}} が使用可能かどうかを確認できます。

フォーラムを参照して、他のユーザーも同じ問題を経験していないか調べることができます。 フォーラムを使用して質問するときは、{{site.data.keyword.cloud_notm}} 開発チームの目に止まるように、質問にタグを付けてください。

- {{site.data.keyword.keymanagementserviceshort}} について技術的な質問がある場合は、[Stack Overflow ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} に質問を投稿し、その質問に「ibm-cloud」と「key-protect」のタグを付けてください。
- サービスや使用開始の手順についての質問には、[IBM developerWorks dW Answers ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} フォーラムを使用してください。 「ibm-cloud」と「key-protect」のタグを付けてください。

フォーラムの使用について詳しくは、[ヘルプの利用 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/docs/support/index.html#getting-help){: new_window} を参照してください。

{{site.data.keyword.IBM_notm}} サポート・チケットのオープンや、サポート・レベルおよびチケットの重大度について詳しくは、[サポートへの問い合わせ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/docs/support/index.html#contacting-support){: new_window} を参照してください。
