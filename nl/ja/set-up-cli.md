---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: install CLI plug-in, install CLI plugin, update CLI plug-in, update CLI plugin, uninstall CLI plug-in, uninstall CLI plugin, Key Protect CLI plug-in, Key Protect CLI plugin, KMS plug-in, KMS plugin

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

# CLI のセットアップ
{: #set-up-cli}

{{site.data.keyword.keymanagementservicelong_notm}} CLI プラグインを使用して、暗号鍵の作成、インポート、および管理に役立てることができます。

この {{site.data.keyword.keymanagementserviceshort}} CLI プラグインの使用について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} CLI リファレンス資料](/docs/services/key-protect?topic=key-protect-cli-reference)を確認してください。
{: tip}

## {{site.data.keyword.keymanagementserviceshort}} CLI プラグインのインストール
{: #install-cli}

{{site.data.keyword.keymanagementserviceshort}} CLI プラグインをセットアップするには、その前に、[{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-overview){: new_window} をインストールしておく必要があります。 

CLI をインストールするには、以下のようにします。

1. [{{site.data.keyword.cloud_notm}} CLI ![External linkicon](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-overview){: new_window} をインストールします。

    CLI のインストール後、`ibmcloud` コマンドを実行して、クラウド・サービスと対話できます。

2. {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

3. 暗号鍵の管理を開始するには、{{site.data.keyword.keymanagementserviceshort}} CLI プラグインをインストールします。

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. オプション: プラグインが正常にインストールされたことを確認します。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## {{site.data.keyword.keymanagementserviceshort}} CLI プラグインの更新
{: #update-cli}

新しいフィーチャーを使用するためには、CLI を定期的に更新する必要があります。

CLI を更新するには、以下のようにします。

1. [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-overview){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

2. プラグイン・リポジトリーから更新をインストールします。

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. オプション: プラグインが正常に更新されたことを確認します。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## {{site.data.keyword.keymanagementserviceshort}} CLI プラグインのアンインストール
{: #uninstall-cli}

1. [{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-overview){: new_window} を使用して {{site.data.keyword.cloud_notm}} にログインします。

    ```sh
    ibmcloud login 
    ```
    {: pre}

    ログインに失敗した場合は、`ibmcloud login --sso` コマンドを実行して、再試行してください。 フェデレーテッド ID を使用してログインする場合は、`--sso` パラメーターが必要です。 このオプションを使用する場合、CLI 出力にリストされたリンクにアクセスして、ワンタイム・パスコードを生成します。
    {: note}

2. プラグイン・リポジトリーから更新をインストールします。

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. オプション: プラグインが正常にアンインストールされたことを確認します。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
