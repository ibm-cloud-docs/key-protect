---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# 액세스 토큰 검색
{: #retrieve-access-token}

{{site.data.keyword.iamlong}}(IAM) 액세스 토큰으로 서비스에 대한 요청을 인증하여 {{site.data.keyword.keymanagementservicelong}} API를 시작하십시오.
{: shortdesc}

## CLI를 사용하여 액세스 토큰 검색
{: #retrieve-token-cli}

[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}를 사용하여 개인용 Cloud IAM 액세스 토큰을 신속하게 생성할 수 있습니다.

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.
    {: note}

2. {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스가 포함된 계정, 지역 및 리소스 그룹을 선택하십시오.

3. 다음 명령을 실행하여 Cloud IAM 액세스 토큰을 검색하십시오.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    다음 잘린 예는 검색된 IAM 토큰을 표시합니다.

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## API를 사용하여 액세스 토큰 검색
{: #retrieve-token-api}

먼저 애플리케이션의 [서비스 ID API 키](/docs/iam?topic=iam-serviceidapikeys)를 작성한 다음 API 키를 {{site.data.keyword.cloud_notm}} IAM 토큰으로 교환하여 액세스 토큰을 프로그래밍 방식으로 검색할 수도 있습니다.

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.
    {: note}

2. {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스가 포함된 계정, 지역 및 리소스 그룹을 선택하십시오.

3. 애플리케이션의 [서비스 ID](/docs/iam?topic=iam-serviceids#creating-a-service-id)를 작성하십시오.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. 서비스 ID에 대한 [액세스 정책을 지정](/docs/iam?topic=iam-serviceidpolicy)하십시오.

    [{{site.data.keyword.cloud_notm}} 콘솔을 사용하여](/docs/iam?topic=iam-serviceidpolicy#access_new) 서비스 ID에 액세스 권한을 지정할 수 있습니다. _관리자_, _작성자_ 및 _독자_ 액세스 역할을 특정 {{site.data.keyword.keymanagementserviceshort}} 서비스 조치에 맵핑하는 방법에 대해 알아보려면 [역할 및 권한](/docs/services/key-protect?topic=key-protect-manage-access#roles)을 참조하십시오.
    {: tip}

5. [서비스 ID API 키](/docs/iam?topic=iam-serviceidapikeys)를 작성하십시오.

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  `<service_ID_name>`을 이전 단계에서 서비스 ID에 지정한 고유 별명으로 바꾸십시오. 보안 위치로 API 키를 다운로드하여 저장하십시오. 

6. [IAM ID 서비스 API](https://{DomainName}/apidocs/iam-identity-token-api)를 호출하여 액세스 토큰을 검색하십시오.

    ```cURL
    curl -X POST \
      "https://iam.cloud.ibm.com/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" > token.json
    ```
    {: codeblock}

    요청에서 `<API_KEY>`를 이전 단계에서 작성한 API 키로 바꾸십시오. 다음 잘린 예는 `token.json` 파일의 컨텐츠를 표시합니다. 

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

    전체 `access_token` 값(_Bearer_ 토큰 유형이 접두부임)을 사용하여 {{site.data.keyword.keymanagementserviceshort}} API로 서비스의 키를 프로그래밍 방식으로 관리하십시오. {{site.data.keyword.keymanagementserviceshort}} API 요청 예를 보려면 [API 요청 구성](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request)을 참조하십시오.

    액세스 토큰은 1시간 동안 유효하지만 필요에 따라 재생성할 수 있습니다. 서비스에 대한 액세스를 유지보수하려면 [IAM ID 서비스 API](https://{DomainName}/apidocs/iam-identity-token-api)를 호출하여 정기적으로 API 키에 대한 액세스 토큰을 재생성하십시오.   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->
