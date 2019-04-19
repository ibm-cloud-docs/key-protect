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

# 擷取存取記號
{: #retrieve-access-token}

使用 {{site.data.keyword.iamlong}} (IAM) 存取記號來鑑別您的服務要求，以開始使用 {{site.data.keyword.keymanagementservicelong}} API。
{: shortdesc}

## 使用 CLI 擷取存取記號
{: #retrieve-token-cli}

您可以使用 [{{site.data.keyword.cloud_notm}} CLI ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} 來快速產生您的個人 Cloud IAM 存取記號。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}，登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

2. 選取包含 {{site.data.keyword.keymanagementserviceshort}} 之佈建實例的帳戶、地區和資源群組。

3. 執行下列指令，以擷取 Cloud IAM 存取記號。

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    下列縮減的範例顯示擷取的 IAM 記號。

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## 使用 API 擷取存取記號
{: #retrieve-token-api}

您也可以程式設計方式來擷取存取記號，方法是先為您的應用程式建立一個[服務 ID API 金鑰](/docs/iam?topic=iam-serviceidapikeys)，然後交換 {{site.data.keyword.cloud_notm}} IAM 記號的 API 金鑰。

1. 使用 [{{site.data.keyword.cloud_notm}} CLI ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}，登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login
    ```
    {: pre}

    如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。
    {: note}

2. 選取包含 {{site.data.keyword.keymanagementserviceshort}} 之佈建實例的帳戶、地區和資源群組。

3. 為您的應用程式建立一個[服務 ID](/docs/iam?topic=iam-serviceids#creating-a-service-id)。

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [指派服務 ID 的存取原則](/docs/iam?topic=iam-serviceidpolicy)。

    您可以[使用 {{site.data.keyword.cloud_notm}} 主控台](/docs/iam?topic=iam-serviceidpolicy#access_new)來指派服務 ID 的存取許可權。若要瞭解_管理員_、_作者_ 及_讀者_ 存取角色如何對映至特定的 {{site.data.keyword.keymanagementserviceshort}} 服務動作，請參閱[角色及許可權](/docs/services/key-protect?topic=key-protect-manage-access#roles)。
    {: tip}

5. 建立一個[服務 ID API 金鑰](/docs/iam?topic=iam-serviceidapikeys)。

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  將 `<service_ID_name>` 取代為您在前一個步驟中指派給服務 ID 的唯一別名。請將 API 金鑰下載至安全位置來進行儲存。 

6. 呼叫 [IAM Identity Services API](https://{DomainName}/apidocs/iam-identity-token-api) 來擷取您的存取記號。

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>"
    ```
    {: codeblock}

    在要求中，將 `<API_KEY>` 取代為您在前一個步驟中建立的 API 金鑰。下列縮減的範例顯示記號輸出：

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

    使用前面加上 _Bearer_ 記號類型的完整 `access_token` 值，以程式設計方式使用 {{site.data.keyword.keymanagementserviceshort}} API 來管理服務的金鑰。若要查看 {{site.data.keyword.keymanagementserviceshort}} API 要求範例，請參閱[形成 API 要求](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request)。

    存取記號的有效時間為 1 小時，但您可以視需要重新產生它們。若要維護對服務的存取，請呼叫 [IAM Identity Services API](https://{DomainName}/apidocs/iam-identity-token-api)，以定期重新產生 API 金鑰的存取記號。   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->
