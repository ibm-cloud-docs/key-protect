---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# アクセス・トークンの取得
{: #retrieve-access-token}

{{site.data.keyword.keymanagementservicelong}} API の使用を開始するため、まず、{{site.data.keyword.iamlong}} (IAM) アクセス・トークンを使用してこのサービスへの要求を認証します。
{: shortdesc}

## CLI を使用したアクセス・トークンの取得
{: #retrieve-token-cli}

[{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して、個人用 Cloud IAM アクセス・トークンを迅速に生成できます。

1. [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

2. プロビジョン済みの {{site.data.keyword.keymanagementserviceshort}} インスタンスを含んでいる、アカウント、地域、およびリソース・グループを選択します。

3. 以下のコマンドを実行して、Cloud IAM アクセス・トークンを取得します。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    以下の省略された例は、取得した IAM トークンを示しています。

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## API を使用したアクセス・トークンの取得
{: #retrieve-token-api}

アクセス・トークンは、アプリケーションの [サービス ID API 鍵](/docs/iam?topic=iam-serviceidapikeys)を作成してから、{{site.data.keyword.cloud_notm}} IAM トークン用に API 鍵を交換することによって、プログラマチックに取得することもできます。

1. [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

2. プロビジョン済みの {{site.data.keyword.keymanagementserviceshort}} インスタンスを含んでいる、アカウント、地域、およびリソース・グループを選択します。

3. アプリケーションの[サービス ID](/docs/iam?topic=iam-serviceids#creating-a-service-id) を作成します。

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. サービス ID に対する[アクセス・ポリシーの割り当て](/docs/iam?topic=iam-serviceidpolicy)を行います。

    [{{site.data.keyword.cloud_notm}} コンソールの使用](/docs/iam?topic=iam-serviceidpolicy#access_new)によって、サービス ID に対するアクセス権限を割り当てることができます。 _管理者_、_ライター_、および_リーダー_ のアクセス役割が特定の {{site.data.keyword.keymanagementserviceshort}} サービス・アクションにどのように対応しているのかについては、[役割と許可](/docs/services/key-protect?topic=key-protect-manage-access#roles)を参照してください。
    {: tip}

5. [サービス ID API 鍵](/docs/iam?topic=iam-serviceidapikeys)を作成します。

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  `<service_ID_name>` を、前のステップでサービス ID に割り当てた固有の別名に置き換えてください。 API キーをダウンロードして、安全な場所に保存します。 

6. [IAM Identity Services API](https://{DomainName}/apidocs/iam-identity-token-api) を呼び出して、アクセス・トークンを取得します。

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>"
    ```
    {: codeblock}

    要求内の `<API_KEY>` を、前のステップで作成した API 鍵に置き換えてください。 次の省略された例に、 トークン出力を示しています。

    ```
    {
    "access_token": "eyJraWQiOiIyM...",
    "expiration": 1512161390,
    "expires_in": 3600,
    "refresh_token": "...",
    "token_type": "Bearer"
    }
    ```
    {: screen}

    完全な `access_token` 値 (接頭部に _Bearer_ トークン・タイプが付いている) を使用して、サービス用の鍵を {{site.data.keyword.keymanagementserviceshort}} API を使ってプログラムで管理します。 {{site.data.keyword.keymanagementserviceshort}} API 要求のサンプルを見るには、[API 要求の作成](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request)を参照してください。

    アクセス・トークンが有効なのは 1 時間ですが、必要に応じて再生成できます。 サービスへのアクセス権限を維持するには、[IAM Identity Services API](https://{DomainName}/apidocs/iam-identity-token-api) を呼び出すことによって、定期的に API 鍵のアクセス・トークンを再生成してください。   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->
