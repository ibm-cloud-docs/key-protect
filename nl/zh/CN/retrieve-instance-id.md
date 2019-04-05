---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

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

# 检索实例标识
{: #retrieve-instance-ID}

您可以将单个 {{site.data.keyword.keymanagementservicelong}} 服务实例作为目标进行操作，方法是在针对该服务的 API 请求中包含其唯一标识或实例标识。
{: shortdesc}

## 在 {{site.data.keyword.cloud_notm}} 控制台中查看实例标识
{: #view-instance-ID}

可以通过导航到 {{site.data.keyword.cloud_notm}} 资源列表来查看与 {{site.data.keyword.keymanagementserviceshort}} 服务实例关联的实例标识。

1. [登录到 {{site.data.keyword.cloud_notm}} 控制台 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
2. 转至**菜单** &gt; **资源列表**，然后单击**服务**来浏览云服务列表。
3. 单击描述 {{site.data.keyword.keymanagementserviceshort}} 服务实例的表行。
4. 从服务详细视图中，复制 **GUID** 值。

    此 **GUID** 值表示用于唯一标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的实例标识。

## 使用 CLI 检索实例标识
{: #retrieve-instance-ID-cli}

也可以使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 来检索服务实例的实例标识。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

2. 选择包含供应的 {{site.data.keyword.keymanagementserviceshort}} 实例的帐户、区域和资源组。

3. 检索唯一标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的云资源名称 (CRN)。 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    将 `<instance_name>` 替换为指定给 {{site.data.keyword.keymanagementserviceshort}} 服务实例的唯一别名。以下截断的示例显示 CLI 输出。

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    _42454b3b-5b06-407b-a4b3-34d9ef323901_ 值是示例实例标识。


## 使用 API 检索实例标识
{: #retrieve-instance-ID-api}

您可能要以编程方式检索实例标识，以帮助您构建并连接应用程序。可以先调用 [{{site.data.keyword.cloud_notm}} 资源控制器 API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/resource-controller)，然后再将 JSON 输出传送到 `jq` 来抽取此值。

1. [检索 {{site.data.keyword.cloud_notm}} IAM 访问令牌](/docs/services/key-protect?topic=key-protect-retrieve-access-token)。
2. 请调用[资源控制器 API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/resource-controller) 来检索实例标识。

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    将 `<instance_name>` 替换为指定给 {{site.data.keyword.keymanagementserviceshort}} 服务实例的唯一别名。以下输出显示实例标识示例：

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
