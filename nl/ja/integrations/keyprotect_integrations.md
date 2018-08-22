---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# サービスの統合
{: #integrations}

{{site.data.keyword.keymanagementservicefull}} は、データおよびストレージのソリューションと統合されて、ユーザー独自の暗号化をクラウドに取り込んで管理するのを支援します。
{: shortdesc}

[サービスのインスタンスを作成した後](/docs/services/keymgmt/keyprotect_provision.html)、{{site.data.keyword.keymanagementserviceshort}} を以下のサポートされているサービスと統合できます。

<table>
    <tr>
        <th>サービス</th>
        <th>説明</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>{{site.data.keyword.keymanagementserviceshort}} を使用して、[エンベロープ暗号化](/docs/services/keymgmt/concepts/keyprotect_envelope.html)をストレージ・バケットに追加します。{{site.data.keyword.keymanagementserviceshort}} で管理するルート鍵を使用して、保存中データを暗号化するデータ暗号鍵を保護します。</p>
          <p>詳しくは、[{{site.data.keyword.cos_full_notm}} との統合](/docs/services/keymgmt/integrations/keyprotect_cloud-object-storage.html)を参照してください。</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} で使用可能な統合についての説明</caption>
</table>

## 統合について 
{: #understand_integration}

サポートされているサービスを {{site.data.keyword.keymanagementserviceshort}} と統合する際、そのサービスに対して [エンベロープ暗号化](/docs/services/keymgmt/concepts/keyprotect_envelope.html)を使用可能にします。この統合によって、{{site.data.keyword.keymanagementserviceshort}} で保管するルート鍵を使用して、保存中データを暗号化するデータ暗号鍵をラップすることが可能になります。 

例えば、ルート鍵を作成し、それを {{site.data.keyword.keymanagementserviceshort}} で管理し、そのルート鍵を使用して、複数のクラウド・サービスで保管されているデータを保護することができます。

![図は、{{site.data.keyword.keymanagementserviceshort}} 統合のコンテキスト・ビューを示しています。](../images/kp-integrations_min.svg)

### {{site.data.keyword.keymanagementserviceshort}} API メソッド
{: #api_methods}

背景では、エンベロープ暗号化プロセスが {{site.data.keyword.keymanagementserviceshort}} API によって駆動されます。  

次の表に、リソースに対するエンベロープ暗号化の追加または削除を行う API メソッドをリストします。

<table>
  <tr>
    <th>メソッド</th>
    <th>説明</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_wrap_keys.html">データ暗号鍵をラップ (暗号化) します</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_unwrap_keys.html">データ暗号鍵をアンラップ (暗号化解除) します</a></td>
  </tr>
  <caption style="caption-side:bottom;">表 2. {{site.data.keyword.keymanagementserviceshort}} API メソッドについての説明</caption>
</table>

{{site.data.keyword.keymanagementserviceshort}} のプログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/apidocs/639){: new_window} に記載されているコード・サンプルを確認してください。
{: tip}

## サポートされるサービスの統合
{: #grant_access}

統合を追加するには、{{site.data.keyword.iamlong}} ダッシュボードを使用してサービス間の許可を作成します。許可によってサービス間のアクセス・ポリシーが有効になるため、クラウド・データ・サービス内のリソースを、{{site.data.keyword.keymanagementserviceshort}} で管理する[ルート鍵](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types)と関連付けることが可能になります。

許可を作成する前に、必ず同じ地域に両方のサービスをプロビジョンしてください。サービスの許可について詳しくは、[サービス間のアクセスの認可 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/iam/authorizations.html){: new_window} を参照してください。
{: tip}

サービスを統合する準備ができたら、以下の手順で許可を作成します。

1. [{{site.data.keyword.cloud_notm}} コンソール ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/){: new_window} にログインします。
2. メニュー・バーで、**「管理」** &gt; **「セキュリティー」** &gt; **「ID およびアクセス」** をクリックして、**「許可」**を選択します。 
3. **「作成」**をクリックします。
4. 許可のソースとターゲットを選択します。
 
  - **「ソース・サービス」**に、{{site.data.keyword.keymanagementserviceshort}} と統合するクラウド・データ・サービスを選択します。例えば、**Cloud Object Storage** です。
  - **「ターゲット・サービス」**に、**「{{site.data.keyword.keymanagementservicelong_notm}}」**を選択します。 
4. サービス間に読み取り専用アクセス権限を付与するには、**「読者」**チェック・ボックスを選択します。

_読者_ の許可がある場合、ソース・サービスは、{{site.data.keyword.keymanagementserviceshort}} の指定のインスタンス内でプロビジョンされたルート鍵を参照できます。
5. **「許可」**をクリックします。

### 次に行うこと

{{site.data.keyword.keymanagementserviceshort}} でルート鍵を作成することによって、拡張暗号化をクラウド・リソースに追加します。サポートされるクラウド・データ・サービスに新規リソースを追加し、次に、拡張暗号化に使用するルート鍵を選択します。

- {{site.data.keyword.keymanagementserviceshort}} サービスでのルート鍵の作成について詳しくは、[ルート鍵の作成](/docs/services/keymgmt/keyprotect_create_root.html)を参照してください。
- {{site.data.keyword.keymanagementserviceshort}} サービスへの独自のルート鍵の取り込みについて詳しくは、[ルート鍵のインポート](/docs/services/keymgmt/keyprotect_import_root.html)を参照してください。


