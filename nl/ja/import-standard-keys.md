---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import standard encryption key, upload standard encryption key, import secret, persist secret, store secret, upload secret, store encryption key, standard key API examples

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

# 標準鍵のインポート
{: #import-standard-keys}

{{site.data.keyword.keymanagementserviceshort}} GUI を使用して、あるいは {{site.data.keyword.keymanagementserviceshort}} API を使用してプログラムで、既存の暗号鍵を追加することができます。

## GUI を使用した標準鍵のインポート
{: #import-standard-key-gui}

[サービスのインスタンスを作成した後](/docs/services/key-protect?topic=key-protect-provision)、以下の手順を実行して、{{site.data.keyword.keymanagementserviceshort}} GUI で既存の標準鍵を入力します。

1. [{{site.data.keyword.cloud_notm}} コンソールにログインします](https://{DomainName}/){: external}。
2. **「メニュー」**&gt;**「リソース・リスト」**に移動し、リソースのリストを表示します。
3. {{site.data.keyword.cloud_notm}} リソース・リストで、{{site.data.keyword.keymanagementserviceshort}} のプロビジョン済みインスタンスを選択します。
4. 新しい鍵をインポートするには、**「鍵の追加」**をクリックして、**「独自の鍵をインポート (Import your own key)」**ウィンドウを選択します。

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
        <td>{{site.data.keyword.keymanagementserviceshort}} で管理する<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">鍵のタイプ</a>。 鍵のタイプのリストから、<b>「標準鍵」</b>を選択します。</td>
      </tr>
      <tr>
        <td>鍵の素材</td>
        <td>
          <p>サービスで管理する、Base64 エンコード鍵の素材 (対称鍵など)。</p>
          <p>鍵の素材が以下の要件に適合していることを確認してください。</p>
          <p><ul>
              <li>鍵の最大サイズは 10,000 バイトです。</li>
              <li>データのバイトは Base64 エンコードでなければなりません。</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">表 1. <b>「独自の鍵をインポート (Import your own key)」</b>の設定の説明</caption>
    </table>

5. 鍵の詳細の記入が完了したら、**「鍵のインポート (Import key)」**をクリックして確認します。 

## API を使用した標準鍵のインポート
{: #import-standard-key-api}

以下のエンドポイントへの `POST` 呼び出しを行うことにより、標準鍵をインポートします。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [サービス内で鍵の処理を行うために、サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 以下の cURL コマンドで [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect){: external} を呼び出します。

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
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
       "extractable": <key_type>
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
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
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
        <td><varname>return_preference</varname></td>
        <td><p><code>POST</code> および <code>DELETE</code> の操作に関するサーバーの動作を変更するヘッダー。</p><p><em>return_preference</em> 変数を <code>return=minimal</code> に設定すると、サービスは鍵のメタデータ (鍵の名前や ID 値など) のみを応答のエンティティー本体で返します。 変数を <code>return=representation</code> に設定すると、サービスは鍵の素材と鍵のメタデータの両方を返します。</p></td>
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
        <td>システム内の鍵の有効期限が切れる日時 (RFC 3339 形式)。 <code>expirationDate</code> 属性を省略した場合、鍵の有効期限は切れません。 </td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>サービスで管理する、Base64 エンコード鍵の素材 (対称鍵など)。</p>
          <p>鍵の素材が以下の要件に適合していることを確認してください。</p>
          <p><ul>
              <li>鍵の最大サイズは 10,000 バイトです。</li>
              <li>データのバイトは Base64 エンコードでなければなりません。</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>鍵の素材をサービスの外に出すことができるかどうかを決定するブール値。</p>
          <p><code>extractable</code> 属性を <code>true</code> に設定すると、サービスはその鍵を、アプリやサービスに保管できる標準鍵として指定します。</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 2. {{site.data.keyword.keymanagementserviceshort}} API を使用して標準鍵を追加するために必要な変数についての説明</caption>
    </table>

    個人データの機密性を保護するため、サービスに鍵を追加するときに、個人の名前や場所などの個人情報 (PII) を入力しないようにしてください。 その他の PII の例については、[NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external} のセクション 2.2 を参照してください。
    {: important}

    成功した `POST api/v2/keys` 応答は、鍵の ID 値を他のメタデータと共に返します。 この ID は、鍵に割り当てられた固有の ID で、{{site.data.keyword.keymanagementserviceshort}} API に対する以降の呼び出しに使用されます。

3. オプション: 次の呼び出しを実行して {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス内の鍵を取得し、鍵が追加されたことを確認します。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}


## 次に行うこと
{: #import-standard-key-next-steps}

- プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料](https://{DomainName}/apidocs/key-protect){: external}を確認してください。
