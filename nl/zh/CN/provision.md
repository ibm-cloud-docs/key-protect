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

# 供应服务
{: #provision}

您可以使用 {{site.data.keyword.cloud_notm}} 控制台或 {{site.data.keyword.cloud_notm}} CLI 创建 {{site.data.keyword.keymanagementservicefull}} 实例。
{: shortdesc}

## 从 {{site.data.keyword.cloud_notm}} 控制台供应
{: #provision-gui}

要从 {{site.data.keyword.cloud_notm}} 控制台供应 {{site.data.keyword.keymanagementserviceshort}} 的实例，请完成以下步骤。

1. [登录到 {{site.data.keyword.cloud_notm}} 帐户 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
2. 单击**目录**以查看 {{site.data.keyword.cloud_notm}} 上可用的服务的列表。
3. 在“所有类别”导航窗格中，单击**安全性和身份**类别。
4. 从服务列表中，单击 **{{site.data.keyword.keymanagementserviceshort}}** 磁贴。
5. 选择服务套餐，然后单击**创建**以在您登录到的帐户、区域和资源组中供应 {{site.data.keyword.keymanagementserviceshort}} 实例。

## 从 {{site.data.keyword.cloud_notm}} CLI 供应
{: #provision-cli}

您也可以使用 {{site.data.keyword.cloud_notm}} CLI 来供应 {{site.data.keyword.keymanagementserviceshort}} 的实例。 

1. 通过 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}
    **注：**如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。

2. 选择要在其中创建 {{site.data.keyword.keymanagementserviceshort}} 服务实例的帐户、区域和资源组。

    可以使用以下命令来设置目标区域和资源组。

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. 在该帐户和资源组内供应 {{site.data.keyword.keymanagementserviceshort}} 实例。

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    将 `<instance_name>` 替换为服务实例的唯一别名。

4. 可选：验证是否已成功创建服务实例。

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## 后续工作
{: #provision-service-next-steps}

要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
