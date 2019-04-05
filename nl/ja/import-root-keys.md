---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# ルート鍵のインポート
{: #import-root-keys}

{{site.data.keyword.keymanagementservicefull}} では、{{site.data.keyword.keymanagementserviceshort}} GUI を使用して、あるいは {{site.data.keyword.keymanagementserviceshort}} API を使用してプログラムで、既存のルート鍵を保護することができます。
{: shortdesc}

ルート鍵は、クラウド内の暗号化データのセキュリティーを保護するために使用される対称鍵ラップ鍵です。 {{site.data.keyword.keymanagementserviceshort}} へのルート鍵のインポートについて詳しくは、[クラウドへの独自の暗号鍵の取り込み](/docs/services/key-protect?topic=key-protect-importing-keys)を参照してください。

## GUI を使用したルート鍵のインポート
{: #import-root-key-gui}

[サービスのインスタンスを作成した後](/docs/services/key-protect?topic=key-protect-provision)、以下の手順を実行して、{{site.data.keyword.keymanagementserviceshort}} GUI で既存のルート鍵を追加します。

1. [{{site.data.keyword.cloud_notm}} コンソール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にログインします](https://{DomainName}/){: new_window}。
2. **「メニュー」**&gt;**「リソース・リスト」**に移動し、リソースのリストを表示します。
3. {{site.data.keyword.cloud_notm}} リソース・リストで、{{site.data.keyword.keymanagementserviceshort}} のプロビジョン済みインスタンスを選択します。
4. 鍵をインポートするには、**「鍵の追加」**をクリックして、**「独自の鍵をインポート (Import your own key)」**ウィンドウを選択します。

    鍵の詳細を以下のように指定します。

    <table>
      <tr>
        <th>設定</th>
        <th>説明</th>
      </tr>
      <tr>
        <td>名前</td>
        <td>
          <p>鍵を簡単に識別するための、人間が理解できる固有の別名。</p>
          <p>プライバシーを保護するため、鍵の名前には、個人の名前や場所などの個人情報 (PII) を含めないように注意してください。</p>
        </td>
      </tr>
      <tr>
        <td>鍵のタイプ</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} で管理する<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">鍵のタイプ</a>。鍵のタイプのリストから、<b>「ルート鍵」</b>を選択します。</td>
      </tr>
      <tr>
        <td>鍵の素材</td>
        <td>
          <p>サービスで保管および管理する、base64 エンコードの鍵素材 (既存の鍵ラップ鍵など)。</p>
          <p>鍵の素材が以下の要件に適合していることを確認してください。</p>
          <p>
            <ul>
              <li>鍵は 128 ビット、192 ビット、または 256 ビットでなければならない。</li>
              <li>データのバイト数 (例えば、256 ビットの場合は 32 バイト) は、base64 エンコードを使用してエンコードされていなければならない。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. <b>「独自の鍵をインポート (Import your own key)」</b>の設定の説明</caption>
    </table>

5. 鍵の詳細の記入が完了したら、**「鍵のインポート (Import key)」**をクリックして確認します。 

## API を使用したルート鍵のインポート
{: #import-root-key-api}

{{site.data.keyword.keymanagementserviceshort}} API を使用して、ルート鍵をサービスにインポートできます。

[鍵素材の作成と暗号化のオプションの検討](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)によって、鍵のインポートについて事前に計画を立ててください。セキュリティーを強化するため、鍵素材をクラウドに取り込む前に[トランスポート鍵](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys)を使用して暗号化することによって、鍵素材のセキュアなインポートを可能にすることができます。トランスポート鍵を使用せずにルート鍵をインポートする場合、[ステップ 4](#import-root-key) までスキップしてください。
{: note}

### ステップ 1: トランスポート鍵の作成
{: #create-transport-key}

現在のところ、トランスポート鍵はベータ・フィーチャーです。ベータ・フィーチャーはいつでも変更される可能性があり、将来の更新で最新バージョンと非互換になるような変更が行われる可能性があります。
{: important}

以下のエンドポイントへの `POST` 呼び出しを行うことによって、サービス・インスタンス用のトランスポート鍵を作成します。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [サービス内で鍵の処理を行うために、サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. [{{site.data.keyword.keymanagementserviceshort}} API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を呼び出すことによって、トランスポート鍵のポリシーを設定します。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

 次の表に従って、例の要求内の変数を置き換えてください。

  <table>
    <tr>
      <th>変数</th>
      <th>説明</th>
    </tr>
    <tr>
      <td><varname>region</varname></td>
      <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
    </tr>
    <tr>
      <td><varname>IAM_token</varname></td>
      <td><strong>必須。</strong> {{site.data.keyword.cloud_notm}} アクセス・トークン。 Bearer 値を含む、<code>IAM</code> トークンの全コンテンツを cURL 要求に組み込みます。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">アクセス・トークンの取得</a>を参照してください。</td>
    </tr>
    <tr>
      <td><varname>instance_ID</varname></td>
      <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てられた固有 ID。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">インスタンス ID の取得</a>を参照してください。</td>
    </tr>
    <tr>
      <td><varname>expiration_time</varname></td>
      <td>
        <p>鍵が有効である期間を示す、トランスポート鍵の作成以降の秒数。</p>
        <p>最小値は 300 秒 (5 分)、最大値は 86400 (24 時間) です。デフォルト値は 600 (10 分) です。</p>
      </td>
    </tr>
    <tr>
      <td><varname>use_count</varname></td>
      <td>トランスポート鍵を有効期間内に取得できる回数。これを超えるとアクセスできなくなります。デフォルト値は 1 です。</td>
    </tr>
      <caption style="caption-side:bottom;">表 2. {{site.data.keyword.keymanagementserviceshort}} API を使用してトランスポート鍵を作成するために必要な変数についての説明</caption>
  </table>

  `POST api/v2/lockers` が成功すると、トランスポート鍵の ID 値および他のメタデータが応答で返されます。この ID は、トランスポート鍵に関連付けられた固有の ID であり、{{site.data.keyword.keymanagementserviceshort}} API への以降の呼び出しで使用されます。

### ステップ 2: トランスポート鍵およびインポート・トークンの取得
{: #retrieve-transport-key}

以下のエンドポイントへの `GET` 呼び出しを行うことによって、トランスポート鍵およびインポート・トークンを取得します。

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. 以下の cURL コマンドを使用して [{{site.data.keyword.keymanagementserviceshort}} API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を呼び出します。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
    ```
    {: codeblock}

    次の表に従って、例の要求内の変数を置き換えてください。
    
    <table>
      <tr>
        <th>変数</th>
        <th>説明</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.cloud_notm}} アクセス・トークン。 Bearer 値を含む、<code>IAM</code> トークンの全コンテンツを cURL 要求に組み込みます。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">アクセス・トークンの取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てられた固有 ID。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">インスタンス ID の取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>locker_ID</varname></td>
        <td><strong>必須。</strong> <a href="#create-transport-key">ステップ 1</a> で作成したトランスポート鍵の ID。</td>
      </tr>
        <caption style="caption-side:bottom;">表 3. {{site.data.keyword.keymanagementserviceshort}} API を使用してトランスポート鍵を取得するために必要な変数についての説明</caption>
    </table>

    `GET api/v2/lockers/{id}` が成功すると、その応答では、4096 ビットの DER エンコードの公開暗号鍵が PKIX 形式で返されます。これを、トランスポート鍵の保全性を検証するために使用されるインポート・トークンと共に、ルート鍵素材を暗号化するために使用できます。

### ステップ 3: 鍵素材の暗号化
{: #encrypt-root-key}

トランスポート鍵を取得した後、その鍵を使用して、{{site.data.keyword.keymanagementserviceshort}} にインポートする鍵素材を暗号化します。  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

オンプレミスで鍵素材を生成するには、[対称暗号鍵の作成のオプションを検討](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)してください。例えば、オンプレミスのハードウェア・セキュリティー・モジュール (HSM) に裏付けられた、組織の内部鍵管理システムを使用して、鍵素材の作成とエクスポートを行いたい場合などが考えられます。
{: note}

鍵素材を暗号化するには、次のようにします。

1. オンプレミス鍵管理システムから、256 ビットの鍵素材をバイナリー形式でエクスポートします。

    鍵素材の作成およびエクスポートの方法について詳しくは、ご使用のオンプレミス HSM または鍵管理システムの資料を参照してください。

2. ステップ 2 で[取得したトランスポート鍵](#retrieve-transport-key)を使用して、鍵素材を暗号化します。

   鍵素材を暗号化するときには、`RSAES_OAEP_SHA_256` 暗号化方式を使用してください。これは、{{site.data.keyword.keymanagementserviceshort}} がトランスポート鍵の作成に使用するデフォルトの方式です。{{site.data.keyword.keymanagementserviceshort}} での暗号化解除の問題を回避するため、鍵素材に対して RSAES_OAEP 暗号化を実行する際、オプションの `label` パラメーターを組み込まないでください。鍵素材に対する RSA 暗号化の実行方法について詳しくは、ご使用のオンプレミス HSM または鍵管理システムの資料を参照してください。

3. 次のステップに進む前に、暗号化された鍵素材が base64 エンコードされていることを確認します。

### ステップ 4: 鍵素材のインポート
{: #import-root-key}

[鍵素材を暗号化および base64 エンコードした後](#encrypt-root-key)、以下のエンドポイントへの `POST` 呼び出しを行うことによって、ルート鍵を {{site.data.keyword.keymanagementserviceshort}} にインポートします。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. 以下の cURL コマンドを使用して [{{site.data.keyword.keymanagementserviceshort}} API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を呼び出します。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
       }
     ]
    }'
    ```
    {: codeblock}

    次の表に従って、例の要求内の変数を置き換えてください。
    <table>
      <tr>
        <th>変数</th>
        <th>説明</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.cloud_notm}} アクセス・トークン。 Bearer 値を含む、<code>IAM</code> トークンの全コンテンツを cURL 要求に組み込みます。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">アクセス・トークンの取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てられた固有 ID。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">インスタンス ID の取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>トランザクションを追跡し、相互に関連付けるために使用される固有 ID。</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>必須。</strong> 鍵を簡単に識別するための、人間が理解できる固有の名前。 プライバシーを保護するため、個人データを鍵のメタデータとして保管しないでください。</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>鍵の詳しい説明。 プライバシーを保護するため、個人データを鍵のメタデータとして保管しないでください。</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>システム内の鍵の有効期限が切れる日時 (RFC 3339 形式)。 <code>expirationDate</code> 属性を省略した場合、鍵の有効期限は切れません。</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>サービスで保管および管理する、base64 エンコードの鍵素材 (既存の鍵ラップ鍵など)。</p>
          <p>鍵の素材が以下の要件に適合していることを確認してください。</p>
          <p>
            <ul>
              <li>鍵は 128 ビット、192 ビット、または 256 ビットでなければならない。</li>
              <li>データのバイト数 (例えば、256 ビットの場合は 32 バイト) は、base64 エンコードを使用してエンコードされていなければならない。</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>鍵の素材をサービスの外に出すことができるかどうかを決定するブール値。</p>
          <p><code>extractable</code> 属性を <code>false</code> に設定すると、サービスはその鍵を、<code>wrap</code> または <code>unwrap</code> の操作に使用できるルート鍵として指定します。</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td><a href="#encrypt-root-key">鍵素材の暗号化</a>に使用した暗号化方式。現在は <code>RSAES_OAEP_SHA_256</code> がサポートされています。トランスポート鍵およびインポート・トークンを使用せずにルート鍵素材をインポートするには、<code>encryption_algorithm</code> 属性を省略します。</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>トランスポート鍵の活動性と保全性を検証するために使用されるインポート・トークン。トランスポート鍵を使用して鍵素材を暗号化する場合は、<a href="#retrieve-transport-key">ステップ 2</a> で取得したのと同じインポート・トークンを提供する必要があります。トランスポート鍵およびインポート・トークンを使用せずにルート鍵素材をインポートするには、<code>importToken</code> 属性を省略します。</td>
      </tr>
        <caption style="caption-side:bottom;">表 4. {{site.data.keyword.keymanagementserviceshort}} API を使用してルート鍵を追加するために必要な変数についての説明</caption>
    </table>

    個人データの機密性を保護するため、サービスに鍵を追加するときに、個人の名前や場所などの個人情報 (PII) を入力しないようにしてください。 その他の PII の例については、[NIST Special Publication 800-122 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window} のセクション 2.2 を参照してください。
    {: important}

    成功した `POST api/v2/keys` 応答は、鍵の ID 値を他のメタデータと共に返します。 この ID は、鍵に割り当てられた固有の ID で、{{site.data.keyword.keymanagementserviceshort}} API に対する以降の呼び出しに使用されます。

2. オプション: 次の呼び出しを実行して {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス内の鍵を表示し、鍵が追加されたことを確認します。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 次に行うこと
{: #import-root-key-next-steps}

- エンベロープ暗号化を使用した鍵の保護について詳しくは、[鍵のラッピング](/docs/services/key-protect?topic=key-protect-wrap-keys)を確認してください。
- プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を確認してください。
