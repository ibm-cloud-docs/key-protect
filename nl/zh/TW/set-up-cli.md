---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: install CLI plug-in, install CLI plugin, update CLI plug-in, update CLI plugin, uninstall CLI plug-in, uninstall CLI plugin, Key Protect CLI plug-in, Key Protect CLI plugin, KMS plug-in, KMS plugin

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 設定 CLI
{: #set-up-cli}

您可以使用 {{site.data.keyword.keymanagementservicelong_notm}} CLI 外掛程式，以協助您建立、匯入及管理加密金鑰。

若要進一步瞭解如何使用 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式，請參閱 [{{site.data.keyword.keymanagementserviceshort}} CLI 參考資料文件](/docs/services/key-protect?topic=key-protect-cli-reference)。
{: tip}

## 安裝 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式
{: #install-cli}

可以設定 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式之前，請先安裝 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}。 

若要安裝 CLI，請執行下列動作：

1. 安裝 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}。

    安裝 CLI 之後，您可以執行 `ibmcloud` 指令，與雲端服務互動。

2. 登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

3. 若要開始管理加密金鑰，請安裝 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式。

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. 選用項目：確認已順利安裝外掛程式。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## 更新 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式
{: #update-cli}

建議您定期更新 CLI 以使用新特性。

若要更新 CLI，請執行下列動作：

1. 使用 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external} 來登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

2. 從外掛程式儲存庫安裝更新。

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. 選用項目：確認已順利更新外掛程式。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## 解除安裝 {{site.data.keyword.keymanagementserviceshort}} CLI 外掛程式
{: #uninstall-cli}

1. 使用 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external} 來登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

2. 從外掛程式儲存庫安裝更新。

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. 選用項目：確認已順利解除安裝外掛程式。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
