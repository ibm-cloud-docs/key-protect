---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

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
{: #gui}

要从 {{site.data.keyword.cloud_notm}} 控制台供应 {{site.data.keyword.keymanagementserviceshort}} 的实例，请完成以下步骤。

1. [登录到 {{site.data.keyword.cloud_notm}} 帐户 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
2. 单击**目录**以查看 {{site.data.keyword.cloud_notm}} 上可用的服务的列表。
3. 在“所有类别”导航窗格中，单击**安全性和身份**类别。
4. 从服务列表中，单击 **{{site.data.keyword.keymanagementserviceshort}}** 磁贴。
5. 选择服务套餐，然后单击**创建**以在您登录到的帐户、区域和资源组中供应 {{site.data.keyword.keymanagementserviceshort}} 实例。

## 从 {{site.data.keyword.cloud_notm}} CLI 供应
{: #cli}

您可以使用 {{site.data.keyword.cloud_notm}} CLI 来供应 {{site.data.keyword.keymanagementserviceshort}} 的实例。 

### 在帐户内创建服务实例
{: #provision-acct-lvl}

要通过 [{{site.data.keyword.iamlong}} 角色](/docs/iam/users_roles.html#iamusermanrol)简化对加密密钥的访问，可以在帐户内创建 {{site.data.keyword.keymanagementserviceshort}} 服务的一个或多个实例，而无需指定组织和空间。 

1. 通过 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

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

### 在组织和空间内创建服务实例
{: #provision-space-lvl}

要使用 [Cloud Foundry 角色](/docs/iam/cfaccess.html)来管理对加密密钥的访问权，可以在指定的组织和空间内创建 {{site.data.keyword.keymanagementserviceshort}} 服务实例。  

1. 通过 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}
    **注：**如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。

2. 选择要在其中创建 {{site.data.keyword.keymanagementserviceshort}} 服务实例的帐户、区域、组织和空间。

    可以使用以下命令来设置目标区域、组织和空间。

    ```sh
    ibmcloud target -r <region> -o <organization_name> -s <space_name>
    ```
    {: pre}

3. 在该帐户、区域、组织和空间内供应 {{site.data.keyword.keymanagementserviceshort}} 实例。

    ```sh
    ibmcloud service create kms tiered-pricing <instance_name>
    ```
    {: pre}

    将 `<instance_name>` 替换为服务实例的唯一别名。

4. 可选：验证是否已成功创建服务实例。

    ```sh
    ibmcloud service list
    ```
    {: pre}


### 后续工作

- 要查看 {{site.data.keyword.keymanagementserviceshort}} 中存储的密钥如何对数据进行加密和解密的示例，请[查看 GitHub 中的样本应用程序 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}。
- 要了解有关以编程方式管理密钥的更多信息，请[查看 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
