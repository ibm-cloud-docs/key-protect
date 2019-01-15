---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 鍵のラッピング
{: #wrap-keys}

特権ユーザーの場合、{{site.data.keyword.keymanagementservicelong}} API を使用して、ルート鍵を使って暗号鍵を管理および保護することができます。
{: shortdesc}

ルート鍵を使用してデータ暗号鍵 (DEK) をラップすると、{{site.data.keyword.keymanagementserviceshort}} は複数のアルゴリズムの長所を組み合わせて、暗号化データのプライバシーおよび保全性を保護します。  

鍵ラッピングが、クラウド内の保存データのセキュリティー管理にどのように役立つかについては、[エンベロープ暗号化](/docs/services/key-protect/concepts/envelope-encryption.html)を参照してください。

## API を使用した鍵のラッピング
{: #api}

{{site.data.keyword.keymanagementserviceshort}} 内で管理するルート鍵を使用して、指定されたデータ暗号化鍵 (DEK) を保護することができます。

ラッピングのためにルート鍵を提供する場合、ラップ呼び出しが成功できるように、ルート鍵が 256 ビット、384 ビット、または 512 ビットであることを確認してください。 サービス内にルート鍵を作成する場合、{{site.data.keyword.keymanagementserviceshort}} はその HSM から、AES-GCM アルゴリズムによってサポートされている 256 ビット鍵を生成します。
{: note}

[サービス内でルート鍵を指定した後](/docs/services/key-protect/create-root-keys.html)、以下のエンドポイントへの `POST` 呼び出しを行うことにより、拡張暗号化を使用して DEK をラップできます。

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [サービス内で鍵の処理を行うために、サービスおよび認証の資格情報を取得します。](/docs/services/key-protect/access-api.html)

2. 管理および保護する DEK の鍵の素材をコピーします。

    {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスの管理者または作成者の特権を持っている場合、[`GET /v2/keys/<key_ID>` 要求を行うことで、特定の鍵の鍵素材を取得できます](/docs/services/key-protect/view-keys.html#api)。

3. ラッピングに使用するルート鍵の ID をコピーします。

4. 以下の cURL コマンドを実行し、ラップ操作を使用して鍵を保護します。

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    ご使用のアカウントの Cloud Foundry 組織およびスペース内で鍵の処理を行うには、`Bluemix-Instance` を、適切な `Bluemix-org` および `Bluemix-space` のヘッダーに置き換えます。 [詳しくは、{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を参照してください。
    {: tip}

    次の表に従って、例の要求内の変数を置き換えてください。

    <table>
      <tr>
        <th>変数</th>
        <th>説明</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect/regions.html#endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必須。</strong> ラッピングに使用するルート鍵の固有 ID。</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.cloud_notm}} アクセス・トークン。 Bearer 値を含む、<code>IAM</code> トークンの全コンテンツを cURL 要求に組み込みます。 詳しくは、<a href="/docs/services/key-protect/access-api.html#retrieve-token">アクセス・トークンの取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てられた固有 ID。 詳しくは、<a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">インスタンス ID の取得</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>トランザクションを追跡し、相互に関連付けるために使用される固有 ID。</td>
      </tr>
      <tr>
        <td><varname>data_key</varname></td>
        <td>管理および保護する DEK の鍵の素材。 <code>plaintext</code> 値は、base64 エンコードでなければなりません。 新しい DEK を生成するには、<code>plaintext</code> 属性を省略します。 サービスは、ランダム・プレーン・テキスト (32 バイト) を生成してその値をラップしてから、生成された値とラップされた値の両方を応答で返します。</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>鍵をさらにセキュアにするために使用される追加認証データ (AAD)。 各ストリングは、最大 255 文字を保持できます。 サービスに対してラップ呼び出を行ったときに AAD を提供した場合は、後続のアンラップ呼び出し時にも同じ AAD を指定する必要があります。<br></br>重要: {{site.data.keyword.keymanagementserviceshort}} サービスは、追加認証データを保存しません。 AAD を提供する場合は、同じ AAD にアクセスすることや、後続のアンラップ要求時に同じ AAD を提供することが確実にできるようにするため、データを安全な場所に保存してください。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} で指定の鍵をラップするために必要な変数についての説明</caption>
    </table>

    ラップされたデータ暗号鍵 (base64 エンコードの鍵素材を含む) が、応答のエンティティー本体で返されます。 以下の JSON オブジェクトは、返された値の例を示しています。

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    ラップ要求を行う際に `plaintext` 属性を省略した場合、サービスは、生成されたデータ暗号鍵 (DEK) とラップされた DEK の両方を Base64 エンコード形式で返します。

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    <code>plaintext</code> の値はアンラップ DEK を表し、<code>ciphertext</code> の値はラップされた DEK を表します。
    
    {{site.data.keyword.keymanagementserviceshort}} によって自分の代わりに新規データ暗号鍵 (DEK) を生成するようにする場合、ラップ要求で空の本体を渡すこともできます。Base64 エンコードの鍵素材が含まれている生成された DEK が、ラップされた DEK とともに応答のエンティティー本体で返されます。{: tip}
    
