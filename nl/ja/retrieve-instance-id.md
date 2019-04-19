---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# インスタンス ID の取得
{: #retrieve-instance-ID}

{{site.data.keyword.keymanagementservicelong}} サービス・インスタンスの固有 ID (つまりインスタンス ID) をサービスへの API 要求に組み込むことによって、個別のサービス・インスタンスを操作のターゲットにすることができます。
{: shortdesc}

## {{site.data.keyword.cloud_notm}} コンソールでのインスタンス ID の表示
{: #view-instance-ID}

{{site.data.keyword.cloud_notm}} リソース・リストにナビゲートすることによって、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスと関連付けられたインスタンス ID を表示できます。

1. [{{site.data.keyword.cloud_notm}} コンソール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にログインします](https://{DomainName}){: new_window}。
2. **「メニュー」** &gt; **「リソース・リスト」**と進み、**「サービス」**をクリックして、クラウド・サービスのリストを表示します。
3. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを記述している表の行をクリックします。
4. サービス詳細ビューから **GUID** 値をコピーします。

    この **GUID** 値は、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを一意的に識別するインスタンス ID を表します。

## CLI を使用したインスタンス ID の取得
{: #retrieve-instance-ID-cli}

サービス・インスタンスのインスタンス ID を、[{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して取得できます。

1. [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

2. プロビジョン済みの {{site.data.keyword.keymanagementserviceshort}} インスタンスを含んでいる、アカウント、地域、およびリソース・グループを選択します。

3. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを一意的に識別するクラウド・リソース名 (CRN) を取得します。 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    `<instance_name>` を、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てた固有の別名に置き換えます。 以下の省略された例は、CLI 出力を示しています。

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    値 _42454b3b-5b06-407b-a4b3-34d9ef323901_ は、インスタンス ID の例です。


## API を使用したインスタンス ID の取得
{: #retrieve-instance-ID-api}

アプリケーションの作成および接続に利用するために、インスタンス ID をプログラマチックに取得したい場合があります。 [{{site.data.keyword.cloud_notm}} Resource Controller API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/resource-controller) を呼び出し、JSON 出力を `jq` にパイプすることによって、この値を取り出すことができます。

1. [{{site.data.keyword.cloud_notm}} IAM アクセス・トークンを取得します](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. [Resource Controller API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/resource-controller) を呼び出して、インスタンス ID を取得します。

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    `<instance_name>` を、{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに割り当てた固有の別名に置き換えます。 以下の出力はインスタンス ID の例を示しています。

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
