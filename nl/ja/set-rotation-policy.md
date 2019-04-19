---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# ローテーション・ポリシーの設定
{: #set-rotation-policy}

{{site.data.keyword.keymanagementservicefull}} を使用して、ルート鍵の自動ローテーション・ポリシーを設定できます。 
{: shortdesc}

ルート鍵の自動ローテーション・ポリシーを設定すると、鍵の存続期間が定期的に短くなり、その鍵で保護される情報量が制限されます。

ローテーション・ポリシーは、{{site.data.keyword.keymanagementserviceshort}} で生成されたルート鍵に対してのみ作成できます。 ルート鍵を最初にインポートした場合、鍵をローテートするには、 新しい Base64 エンコードの鍵素材を指定する必要があります。 詳しくは、[オンデマンドでのルート鍵のローテート](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys)を参照してください。
{: note}

{{site.data.keyword.keymanagementserviceshort}} での鍵のローテーションのオプションについて詳しくは、[鍵のローテーションのオプションの比較](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)を参照してください。
{: tip}

## GUI を使用したローテーション・ポリシーの管理
{: #manage-policies-gui}

グラフィカル・インターフェースを使用してルート鍵のポリシーを管理したい場合は、{{site.data.keyword.keymanagementserviceshort}} GUI を使用できます。

1. [{{site.data.keyword.cloud_notm}} コンソール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にログインします](https://{DomainName}/){: new_window}。
2. **「メニュー」**&gt;**「リソース・リスト」**に移動し、リソースのリストを表示します。
3. {{site.data.keyword.cloud_notm}} リソース・リストで、{{site.data.keyword.keymanagementserviceshort}} のプロビジョン済みインスタンスを選択します。
4. アプリケーションの詳細ページで、**「鍵 (Keys)」**テーブルを使用して、サービス内の鍵を表示します。
5. 「⋯」アイコンをクリックして、特定の鍵のオプションのリストを開きます。
6. オプション・メニューから、鍵のローテーション・ポリシーを管理するために**「ポリシーの管理」**をクリックします。
7. ローテーションのオプションのリストから、ローテーション頻度 (月単位) を選択します。

    鍵のローテーション・ポリシーが既に存在する場合、鍵の既存ローテーション期間がインターフェースで表示されます。

8. **「ポリシーの作成」**をクリックして、鍵のポリシーを設定します。

指定したローテーション間隔に基づいて鍵をローテートするときになったら、ルート鍵は新しい鍵素材で {{site.data.keyword.keymanagementserviceshort}} によって自動的に置き換えられます。

## API を使用したローテーション・ポリシーの管理
{: #manage-rotation-policies-api}

### ローテーション・ポリシーの表示
{: #view-rotation-policy-api}

概略を把握するため、以下のエンドポイントへの `GET` 呼び出しを行うことによって、ルート鍵と関連付けられたポリシーを表示できます。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 以下の cURL コマンドを実行して、指定した鍵のローテーション・ポリシーを取得します。

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    次の表に従って、例の要求内の変数を置き換えてください。
    <table>
      <tr>
        <th>変数</th>
        <th>説明</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>必須。</strong> 既存のローテーション・ポリシーがあるルート鍵の固有 ID。</td>
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
        <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} API を使用してローテーション・ポリシーを作成するために必要な変数についての説明</caption>
    </table>

    `GET api/v2/keys/{id}/policies` が成功すると、鍵と関連付けられたポリシー詳細が応答で返されます。 以下の JSON オブジェクトは、ルート鍵に既存のローテーション・ポリシーがある場合の応答例を示しています。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    `interval_month` 値は、鍵のローテーション頻度 (月単位) を示します。

### ローテーション・ポリシーの作成
{: #create-rotation-policy-api}

以下のエンドポイントへの `PUT` 呼び出しを行うことによって、ルート鍵のローテーション・ポリシーを作成します。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 以下の cURL コマンドを実行して、指定した鍵のローテーション・ポリシーを作成します。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必須。</strong> ローテーション・ポリシーを作成する対象のルート鍵の固有 ID。</td>
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
        <td><varname>rotation_interval</varname></td>
        <td><strong>必須。</strong> 鍵のローテーション間隔を月単位で示す整数値。 最小値は <code>1</code>、最大値は <code>12</code> です。
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} API を使用してローテーション・ポリシーを作成するために必要な変数についての説明</caption>
    </table>

    `PUT api/v2/keys/{id}/policies` が成功すると、鍵と関連付けられたポリシー詳細が応答で返されます。 以下の JSON オブジェクトは、ルート鍵に既存のローテーション・ポリシーがある場合の応答例を示しています。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### ローテーション・ポリシーの更新
{: #update-rotation-policy-api}

以下のエンドポイントへの `PUT` 呼び出しを行うことにより、既存のルート鍵のポリシーを更新します。

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [サービス資格情報および認証資格情報を取得します](/docs/services/key-protect?topic=key-protect-set-up-api)。

2. 以下の cURL コマンドを実行して、指定した鍵のローテーション・ポリシーを置換します。

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>必須。</strong> ローテーション・ポリシーを置換する対象のルート鍵の固有 ID。</td>
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
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>必須。</strong> 鍵のローテーション間隔を月単位で示す整数値。 最小値は <code>1</code>、最大値は <code>12</code> です。
        </td>
      </tr>
        <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} API を使用してローテーション・ポリシーを作成するために必要な変数についての説明</caption>
    </table>

    `PUT api/v2/keys/{id}/policies` が成功すると、鍵と関連付けられた更新後のポリシー詳細が応答で返されます。 以下の JSON オブジェクトは、ルート鍵のローテーション・ポリシーが更新された場合の応答例を示しています。

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    鍵のポリシー詳細のうち、`interval_month` 値と `updatedat` 値が更新されています。 最初に作成したのとは別のユーザーが鍵のポリシーを更新した場合は、`updatedby` 値も変更され、要求を送信したユーザーの ID が示されます。
