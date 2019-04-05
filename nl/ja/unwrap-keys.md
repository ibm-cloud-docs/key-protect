---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-25"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# 鍵のアンラッピング
{: #unwrap-keys}

特権ユーザーの場合、{{site.data.keyword.keymanagementservicefull}} API を使用して、データ暗号化鍵 (DEK) をアンラップして、内容にアクセスすることができます。 DEK をアンラップすると、暗号化解除して内容の保全性を確認し、元の鍵素材を {{site.data.keyword.cloud_notm}} データ・サービスに返します。
{: shortdesc}

鍵ラッピングが、クラウド内の保存データのセキュリティー管理にどのように役立つかについては、[エンベロープ暗号化を使用したデータ保護](/docs/services/key-protect?topic=key-protect-envelope-encryption)を参照してください。

## API を使用した鍵のアンラッピング
{: #unwrap-key-api}

[サービスに対してラップ呼び出しを行った後](/docs/services/key-protect?topic=key-protect-wrap-keys)、以下のエンドポイントへの `POST` 呼び出しを行うことにより、指定のデータ暗号化鍵 (DEK) をアンラップして、その内容にアクセスできます。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [サービス内で鍵の処理を行うために、サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 最初のラップ要求を実行するために使用したルート鍵の ID をコピーします。

    `GET /v2/keys` 要求を行うか、{{site.data.keyword.keymanagementserviceshort}} GUI で鍵を表示することで、鍵の ID を取得できます。

3. 最初のラップ要求時に返された `ciphertext` 値をコピーします。

4. 次の cURL コマンドを実行して、鍵の素材を暗号化解除して認証します。

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
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
        <td><strong>必須。</strong> {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが存在している地理的領域を表す、地域の省略形 (例: <code>us-south</code> または <code>eu-gb</code>)。 詳しくは、<a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">地域のサービス・エンドポイント</a>を参照してください。</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必須。</strong> 初期ラップ要求に使用したルート鍵の固有 ID。</td>
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
        <td><varname>additional_data</varname></td>
        <td>鍵をさらにセキュアにするために使用される追加認証データ (AAD)。 各ストリングは、最大 255 文字を保持できます。 サービスに対してラップ呼び出しを行ったときに AAD を提供した場合は、アンラップ呼び出し時にも同じ AAD を指定する必要があります。</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>必須。</strong> ラップ操作時に返された <code>ciphertext</code> 値。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} で鍵をアンラップするために必要な変数についての説明</caption>
    </table>

    元の鍵の素材が、応答のエンティティー本体で返されます。 以下の JSON オブジェクトは、返された値の例を示しています。

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}
