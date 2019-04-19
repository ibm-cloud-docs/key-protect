---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# API のセットアップ
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} が提供する REST API を使用すると、任意のプログラミング言語を使用して、暗号鍵の保管、取得、および生成を行うことができます。
{: shortdesc}

## {{site.data.keyword.cloud_notm}} 資格情報の取得
{: #retrieve-credentials}

API で作業するには、サービス資格情報と認証資格情報を生成する必要があります。 

資格情報を収集するには、次のようにします。

1. [{{site.data.keyword.cloud_notm}} IAM アクセス・トークンを生成します](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. [{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを一意的に識別するインスタンス ID を取得します](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID)。

## API 要求の作成
{: #form-api-request}

サービスに対して API 呼び出しを行う場合、{{site.data.keyword.keymanagementserviceshort}} のインスタンスを最初にプロビジョンした方法に応じて、API 要求を構成します。 

要求を作成するには、[地域のサービス・エンドポイント](/docs/services/key-protect?topic=key-protect-regions)と適切な認証資格情報を組み合わせます。 例えば、`us-south` 地域のサービス・インスタンスを作成した場合は、以下のエンドポイントと API ヘッダーを使用して、サービス内の鍵を表示します。

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

`<access_token>` および `<instance_ID>` を、取得したサービス資格情報および認証資格情報に置き換えてください。

何か問題が起こった場合に API 要求を追跡する必要があるときは、 cURL 要求の一部として `-v` フラグを組み込むと、応答ヘッダーに `correlation-id` 値が含まれます。 この値を使用して、デバッグ目的で、要求の相互関連付けおよび追跡を行うことができます。
{: tip} 

## 次に行うこと
{: #set-up-api-next-steps}

Key Protect で暗号鍵の管理を始めるためのすべての設定が整いました。 プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を確認してください。
