---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# トランスポート鍵の作成
{: #create-transport-keys}

最初に {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス用のトランスポート暗号鍵を作成することによって、ルート鍵素材をクラウドにセキュアにインポートできるようになります。
{: shortdesc}

トランスポート鍵は、ルート鍵素材を暗号化して {{site.data.keyword.keymanagementserviceshort}} にセキュアにインポートするために、指定するポリシーに基づいて使用されます。 鍵をクラウドにセキュアにインポートすることについて詳しくは、[クラウドへの独自の暗号鍵の取り込み](/docs/services/key-protect/concepts?topic=key-protect-importing-keys)を参照してください。

現在のところ、トランスポート鍵はベータ・フィーチャーです。 ベータ・フィーチャーはいつでも変更される可能性があり、将来の更新で最新バージョンと非互換になるような変更が行われる可能性があります。
{: important}

## API を使用したトランスポート鍵の作成
{: #create-transport-key-api}

以下のエンドポイントへの `POST` 呼び出しを行うことによって、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスと関連付けられたトランスポート鍵を作成します。

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
          <td><varname>expiration_time</varname></td>
          <td>
            <p>鍵が有効である期間を示す、トランスポート鍵の作成以降の秒数。</p>
            <p>最小値は 300 秒 (5 分)、最大値は 86400 (24 時間) です。 デフォルト値は 600 (10 分) です。</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>トランスポート鍵を有効期間内に取得できる回数。これを超えるとアクセスできなくなります。 デフォルト値は 1 です。</td>
        </tr>
          <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} API を使用してルート鍵を追加するために必要な変数についての説明</caption>
      </table>

    `POST api/v2/lockers` 要求が成功すると、サービス・インスタンス用のトランスポート鍵が作成され、鍵の ID 値およびその他のメタデータが返されます。 この ID は、トランスポート鍵に関連付けられた固有 ID であり、{{site.data.keyword.keymanagementserviceshort}} API への以降の呼び出しで使用されます。

3. オプション: 以下の呼び出しを実行してサービス・インスタンスについてのメタデータを取得することによって、トランスポート鍵が作成されたことを検証します。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 次に行うこと
{: #create-transport-key-next-steps}

- ルート鍵をサービスにインポートするためのトランスポート鍵の使用について詳しくは、[ルート鍵のインポート](/docs/services/key-protect?topic=key-protect-import-root-keys)を参照してください。
- プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を確認してください。
