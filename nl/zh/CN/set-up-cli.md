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

# 设置 CLI
{: #set-up-cli}

您可以使用 {{site.data.keyword.keymanagementservicelong_notm}} CLI 插件来帮助您创建、导入和管理加密密钥。

要了解有关使用 {{site.data.keyword.keymanagementserviceshort}} CLI 插件的更多信息，请查看 [{{site.data.keyword.keymanagementserviceshort}} CLI 参考文档](/docs/services/key-protect?topic=key-protect-cli-reference)。
{: tip}

## 安装 {{site.data.keyword.keymanagementserviceshort}} CLI 插件
{: #install-cli}

在设置 {{site.data.keyword.keymanagementserviceshort}} CLI 插件之前，请先安装 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window}。 

要安装 CLI，请执行以下操作：

1. 安装 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window}。

    安装 CLI 后，可以运行 `ibmcloud` 命令来与云服务进行交互。

2. 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

3. 要开始管理加密密钥，请安装 {{site.data.keyword.keymanagementserviceshort}} CLI 插件。

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. 可选：验证是否已成功安装插件。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## 更新 {{site.data.keyword.keymanagementserviceshort}} CLI 插件
{: #update-cli}

要使用新功能，需要定期更新 CLI。

要更新 CLI，请执行以下操作：

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

2. 从插件存储库中安装更新。

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. 可选：验证是否已成功更新插件。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## 卸载 {{site.data.keyword.keymanagementserviceshort}} CLI 插件
{: #uninstall-cli}

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

2. 从插件存储库中安装更新。

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. 可选：验证是否已成功卸载插件。

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
