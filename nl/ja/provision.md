---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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

# サービスのプロビジョニング
{: #provision}

{{site.data.keyword.cloud_notm}} コンソールまたは {{site.data.keyword.cloud_notm}} CLI を使用して、{{site.data.keyword.keymanagementservicefull}} のインスタンスを作成できます。
{: shortdesc}

## {{site.data.keyword.cloud_notm}} コンソールからのプロビジョニング
{: #provision-gui}

{{site.data.keyword.cloud_notm}} コンソールから {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンするには、以下の手順を実行します。

1. [{{site.data.keyword.cloud_notm}} アカウント ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン") にログインします](https://{DomainName}){: new_window}。
2. **「カタログ」**をクリックして、{{site.data.keyword.cloud_notm}} 上で使用可能なサービスのリストを表示します。
3. 「すべてのカテゴリー」ナビゲーション・ペインで、**「セキュリティーおよび ID」**カテゴリーをクリックします。
4. サービスのリストで、**「{{site.data.keyword.keymanagementserviceshort}}」**タイルをクリックします。
5. サービス・プランを選択し、**「作成」**をクリックして、ログインしているアカウント、地域、およびリソース・グループ内の {{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンします。

## {{site.data.keyword.cloud_notm}} CLI からのプロビジョニング
{: #provision-cli}

また、{{site.data.keyword.cloud_notm}} CLI を使用して、{{site.data.keyword.keymanagementserviceshort}} のインスタンスをプロビジョンすることもできます。 

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

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

## 次に行うこと
{: #provision-service-next-steps}

プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/key-protect){: new_window} を確認してください。
