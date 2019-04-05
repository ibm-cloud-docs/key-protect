---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# クラウドへの独自の暗号鍵の取り込み
{: #importing-keys}

暗号鍵には、鍵を識別するのに役立つメタデータなどの情報サブセットと、データの暗号化および暗号化解除に使用される_鍵素材_ が含まれます。
{: shortdesc}

{{site.data.keyword.keymanagementserviceshort}} を使用して鍵を作成すると、このサービスがユーザーに代わって、クラウド・ベースのハードウェア・セキュリティー・モジュール (HSM) に基づく暗号鍵素材を生成します。しかし、ビジネス要件によっては、ユーザーが社内ソリューションから鍵素材を生成し、次に、鍵を {{site.data.keyword.keymanagementserviceshort}} にインポートして、ご利用のオンプレミスの鍵管理インフラストラクチャーをクラウドへ拡張することが必要な場合があります。

<table>
  <th>利点</th>
  <th>説明</th>
  <tr>
    <td>独自の鍵の取り込み (Bring Your Own Key (BYOK))</td>
    <td>強い鍵をオンプレミスのハードウェア・セキュリティー・モジュール (HSM) から生成することによって、鍵管理手法を完全に制御し、強化する必要があるとします。社内の鍵管理インフラストラクチャーから対称鍵をエクスポートすることを選択する場合、{{site.data.keyword.keymanagementserviceshort}} を使用して、それらの鍵をクラウドにセキュアに取り込むことができます。</td>
  </tr>
  <tr>
    <td>ルート鍵素材のセキュアなインポート</td>
    <td>鍵をクラウドにエクスポートするときには、鍵素材が送信中に確実に保護されることが望まれます。<a href="#transport-keys">トランスポート鍵の使用</a>によって中間者攻撃を防ぐことで、ルート鍵素材を {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスにセキュアにインポートできます。</td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 鍵素材のインポートに関する利点の説明</caption>
</table>


## 鍵素材のインポートについての事前計画
{: #plan-ahead}

ルート鍵素材をサービスにインポートする準備ができたら、以下の考慮事項に留意してください。

<dl>
  <dt>鍵素材の作成のオプションを検討する</dt>
    <dd>セキュリティーのニーズに基づいて、256 ビット対称暗号鍵の作成のオプションを検討します。例えば、FIPS で認定されたオンプレミスのハードウェア・セキュリティー・モジュール (HSM) に裏付けられた社内鍵管理システムを使用して、クラウドに鍵を持ち込む前に鍵素材を生成することが可能です。PoC (概念検証) を実施しようとしている場合は、<a href="https://www.openssl.org/" target="_blank">OpenSSL</a> などの暗号化ツールキットを使用して、テストのニーズに合わせて、{{site.data.keyword.keymanagementserviceshort}} にインポートできる鍵素材を生成することも可能です。</dd>
  <dt>{{site.data.keyword.keymanagementserviceshort}} への鍵素材のインポートのオプションを選択する</dt>
    <dd>環境またはワークロードで必要なセキュリティー・レベルに基づいて、ルート鍵のインポートの 2 つのオプションから選択します。デフォルトでは、{{site.data.keyword.keymanagementserviceshort}} は、Transport Layer Security (TLS) 1.2 プロトコルを使用して、転送中の鍵素材を暗号化します。PoC (概念検証) を実施しようとしている場合、または初めてサービスを試してみようとしている場合は、このデフォルト・オプションを使用して、ルート鍵素材を {{site.data.keyword.keymanagementserviceshort}} にインポートできます。TLS を超えるセキュリティー・メカニズムを必要とするワークロードの場合は、<a href="#transport-keys">トランスポート鍵を使用</a>して、ルート鍵素材の暗号化とサービスへのインポートを行うことも可能です。</dd>
  <dt>鍵素材の暗号化について事前に計画を立てる</dt>
    <dd>トランスポート鍵を使用して鍵素材を暗号化することを選択した場合、鍵素材に対して RSA 暗号化を実行する方法を決定します。<a href="https://tools.ietf.org/html/rfc3447" target="_blank">RSA 暗号化についての PKCS #1 v2.1 標準</a>で指定されているように <code>RSAES_OAEP_SHA_256</code> 暗号化スキームを使用する必要があります。社内の鍵管理システムまたはオンプレミス HSM の機能を確認して、オプションを決定してください。</dd>
  <dt>インポートされた鍵素材のライフサイクルを管理する</dt>
    <dd>鍵素材をサービスにインポートした後、鍵のライフサイクル全体を管理する責任があることに留意してください。{{site.data.keyword.keymanagementserviceshort}} API を使用して、鍵をサービスにアップロードすると決めたときに鍵の有効期限日付を設定できます。ただし、<a href="/docs/services/key-protect?topic=key-protect-rotate-keys">インポートされたルート鍵をローテート</a>したい場合は、既存の鍵を無効にして置き換えるために、新しい鍵素材を生成して提供する必要があります。</dd>
</dl>

## トランスポート鍵の使用
{: #transport-keys}

現在のところ、トランスポート鍵はベータ・フィーチャーです。ベータ・フィーチャーはいつでも変更される可能性があり、将来の更新で最新バージョンと非互換になるような変更が行われる可能性があります。
{: important}

{{site.data.keyword.keymanagementserviceshort}} にインポートする前に鍵素材を暗号化したい場合、{{site.data.keyword.keymanagementserviceshort}} API を使用して、サービス・インスタンス用のトランスポート暗号鍵を作成できます。 

トランスポート鍵は、{{site.data.keyword.keymanagementserviceshort}} の 1 つのリソース・タイプであり、ルート鍵素材をサービス・インスタンスにセキュアにインポートすることを可能にします。トランスポート鍵を使用して鍵素材をオンプレミスで暗号化することによって、指定するポリシーに基づいて、{{site.data.keyword.keymanagementserviceshort}} への転送中のルート鍵を保護できます。例えば、時間および使用回数に基づいて鍵の使用を制限するポリシーをトランスポート鍵に対して設定できます。

### 仕組み
{: #how-transport-keys-work}

サービス・インスタンス用の[トランスポート鍵を作成](/docs/services/key-protect?topic=key-protect-create-transport-keys)すると、{{site.data.keyword.keymanagementserviceshort}} によって 4096 ビット RSA 鍵が生成されます。このサービスによって秘密鍵が暗号化され、その後で公開鍵とインポート・トークンが返されるので、それらをルート鍵素材の暗号化およびインポートのために使用できます。 

{{site.data.keyword.keymanagementserviceshort}} への[ルート鍵のインポート](/docs/services/key-protect?topic=key-protect-import-root-keys#api)を行う準備ができたら、暗号化されたルート鍵素材と、公開鍵の保全性を検証するために使用されるインポート・トークンを提供します。要求を実行するために、{{site.data.keyword.keymanagementserviceshort}} は、サービス・インスタンスと関連付けられた秘密鍵を使用して、暗号化されたルート鍵素材を暗号化解除します。このプロセスは、{{site.data.keyword.keymanagementserviceshort}} で生成したトランスポート鍵のみが、サービスにインポートして保管する鍵素材を暗号化解除できることを保証します。

サービス・インスタンス当たり 1 つのみのトランスポート鍵を作成できます。トランスポート鍵の取得に関する制限について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を参照してください。
{: note} 

### API メソッド
{: #secure-import-api-methods}

これらの作業の裏では、{{site.data.keyword.keymanagementserviceshort}} API がトランスポート鍵の作成処理を駆動します。  

次の表に、ロッカーのセットアップおよびサービス・インスタンス用のトランスポート鍵の作成を行う API メソッドを示します。

<table>
  <tr>
    <th>メソッド</th>
    <th>説明</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">トランスポート鍵を作成します</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">トランスポート鍵メタデータを取得します</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">トランスポート鍵およびインポート・トークンを取得します</a></td>
  </tr>
  <caption style="caption-side:bottom;">表 2. {{site.data.keyword.keymanagementserviceshort}} API メソッドについての説明</caption>
</table>

{{site.data.keyword.keymanagementserviceshort}} のプログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を確認してください。

## 次に行うこと

- サービス・インスタンス用のトランスポート鍵を作成する方法については、[トランスポート鍵の作成](/docs/services/key-protect?topic=key-protect-create-transport-keys)を参照してください。
- サービスへの鍵のインポートについて詳しくは、[ルート鍵のインポート](/docs/services/key-protect?topic=key-protect-import-root-keys)を参照してください。 
