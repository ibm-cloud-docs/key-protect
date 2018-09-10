---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# サービスのプロビジョニング
{: #provision}

{{site.data.keyword.cloud_notm}} コンソールまたは {{site.data.keyword.cloud_notm}} CLI を使用して、{{site.data.keyword.keymanagementservicefull}} のインスタンスを作成できます。
{: shortdesc}

## {{site.data.keyword.cloud_notm}} コンソールからのプロビジョニング
{: #gui}

{{site.data.keyword.cloud_notm}} コンソールから {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンするには、以下の手順を実行します。

1. [{{site.data.keyword.cloud_notm}} アカウント ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/){: new_window} にログインします。
2. **「カタログ」**をクリックして、{{site.data.keyword.cloud_notm}} 上で使用可能なサービスのリストを表示します。
3. 「すべてのカテゴリー」ナビゲーション・ペインで、**「プラットフォーム」**までスクロールし、**「セキュリティー」**カテゴリーをクリックします。
4. サービスのリストで、**「{{site.data.keyword.keymanagementserviceshort}}」**タイルをクリックします。
5. サービス・プランを選択し、**「作成」**をクリックして、ログインしているアカウント、地域、およびリソース・グループ内の {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンします。

## {{site.data.keyword.cloud_notm}} CLI からのプロビジョニング
{: #cli}

{{site.data.keyword.cloud_notm}} CLI を使用して、{{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンできます。 

### アカウント内のサービス・インスタンスの作成
{: #provision-acct-lvl}

[{{site.data.keyword.iamlong}} 役割](/docs/iam/users_roles.html#iamusermanrol)を使用して暗号鍵に簡単にアクセスできるようにするには、アカウント内に {{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを 1 つ以上作成して、組織とスペースを指定しなくても済むようにします。 

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login
    ```
    {: pre}
    **注:** ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。

2. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを作成するアカウント、地域、およびリソース・グループを選択します。

    次のコマンドを使用して、ターゲットの地域およびリソース・グループを設定できます。

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. そのアカウントおよびリソース・グループ内の {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンします。

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    `<instance_name>` を、サービス・インスタンスの固有の別名に置き換えます。

4. オプション: サービス・インスタンスが正常に作成されたことを確認します。

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

### 組織およびスペース内のサービス・インスタンスの作成
{: #provision-space-lvl}

[Cloud Foundry 役割](/docs/iam/cfaccess.html)を使用して暗号鍵へのアクセス権限を管理するには、指定された組織とスペース内に {{site.data.keyword.keymanagementserviceshort}} サービスのインスタンスを作成します。  

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login
    ```
    {: pre}
    **注:** ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。

2. {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを作成するアカウント、地域、組織、およびスペースを選択します。

    次のコマンドを使用して、ターゲットの地域、組織、およびスペースを設定できます。

    ```sh
    ibmcloud target -r <region> -o <organization_name> -s <space_name>
    ```
    {: pre}

3. そのアカウント、地域、組織、およびスペース内の {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンします。

    ```sh
    ibmcloud service create kms tiered-pricing <instance_name>
    ```
    {: pre}

    `<instance_name>` を、サービス・インスタンスの固有の別名に置き換えます。

4. オプション: サービス・インスタンスが正常に作成されたことを確認します。

    ```sh
    ibmcloud service list
    ```
    {: pre}


### 次に行うこと

- {{site.data.keyword.keymanagementserviceshort}} に保管されている鍵を使用してデータを暗号化および暗号化解除する方法の例は、[GitHub にあるサンプル・アプリで確認してください ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}。
- プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/apidocs/kms){: new_window} を確認してください。
