---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

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

# 检索访问令牌
{: #retrieve-access-token}

通过使用 {{site.data.keyword.iamlong}} (IAM) 访问令牌来认证对服务的请求，开始使用 {{site.data.keyword.keymanagementservicelong}} API。
{: shortdesc}

## 使用 CLI 检索访问令牌
{: #retrieve-token-cli}

可以使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 快速生成个人 Cloud IAM 访问令牌。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

2. 选择包含供应的 {{site.data.keyword.keymanagementserviceshort}} 实例的帐户、区域和资源组。

3. 运行以下命令来检索 Cloud IAM 访问令牌。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    以下截断的示例显示检索到的 IAM 令牌。

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## 使用 API 检索访问令牌
{: #retrieve-token-api}

还可以通过先为应用程序创建[服务标识 API 密钥](/docs/iam?topic=iam-serviceidapikeys)，然后用您的 API 密钥交换 {{site.data.keyword.cloud_notm}} IAM 令牌，从而以编程方式检索访问令牌。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/cli?topic=cloud-cli-overview){: new_window} 登录到 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登录失败，请运行 `ibmcloud login --sso` 命令重试。使用联合标识登录时需要 `--sso` 参数。如果使用此选项，请转至 CLI 输出中列出的链接以生成一次性密码。
    {: note}

2. 选择包含供应的 {{site.data.keyword.keymanagementserviceshort}} 实例的帐户、区域和资源组。

3. 为应用程序创建[服务标识](/docs/iam?topic=iam-serviceids#creating-a-service-id)。

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. 为服务标识[分配访问策略](/docs/iam?topic=iam-serviceidpolicy)。

    可以[通过使用 {{site.data.keyword.cloud_notm}} 控制台](/docs/iam?topic=iam-serviceidpolicy#access_new)为服务标识分配访问许可权。要了解有关_管理者_、_写入者_和_读取者_访问角色如何映射到特定 {{site.data.keyword.keymanagementserviceshort}} 服务操作的信息，请参阅[角色和许可权](/docs/services/key-protect?topic=key-protect-manage-access#roles)。
    {: tip}

5. 创建[服务标识 API 密钥](/docs/iam?topic=iam-serviceidapikeys)。

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  将 `<service_ID_name>` 替换为先前步骤中指定给服务标识的唯一别名。通过将 API 密钥下载到安全位置来保存该密钥。 

6. 调用 [IAM 身份服务 API](https://{DomainName}/apidocs/iam-identity-token-api) 来检索访问令牌。

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>"
    ```
    {: codeblock}

    在请求中，将 `<API_KEY>` 替换为先前步骤中创建的 API 密钥。以下截断的示例显示令牌输出：

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

    使用完整的 `access_token` 值（以 _Bearer_ 令牌类型为前缀）通过 {{site.data.keyword.keymanagementserviceshort}} API 以编程方式管理服务的密钥。要参阅 {{site.data.keyword.keymanagementserviceshort}} API 请求示例，请查看[构成 API 请求](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request)。

    访问令牌有效期为 1 小时，但是可根据需要重新生成。要维护对服务的访问，请通过调用 [IAM 身份服务 API](https://{DomainName}/apidocs/iam-identity-token-api) 定期重新生成 API 密钥的访问令牌。   
    {: note }

    
