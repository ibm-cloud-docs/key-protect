---

copyright:
  years: 2017
lastupdated: "2017-12-15"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# API로 키에 대한 액세스 관리
{: #managing-access-api}

{{site.data.keyword.iamlong}}를 사용하면 액세스 정책의 작성과 수정을 통해 암호화 키에 대한 세부 단위의 액세스 제어를 사용할 수 있습니다.
{: shortdesc}

이 페이지에서는 [액세스 관리 API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://iampap.ng.bluemix.net/v1/docs/#!/Access_Policies/){: new_window}를 사용하여 키 리소스에 대한 액세스를 관리하기 위한 다양한 시나리오를 안내합니다.


시작하기 전에:
- [IAM 토큰 및 인스턴스 ID 검색](/docs/services/keymgmt/keyprotect_authentication.html)
- [리소스를 지정하기 위한 키 ID 검색](/docs/services/keymgmt/keyprotect_view_keys.html)
- [액세스 범위를 지정하기 위한 계정 ID 검색](keyprotect_manage_access_api.html#retrieve_account_ID)
- [해당 액세스 권한이 수정될 사용자의 사용자 ID 검색](keyprotect_manage_access_api.html#retrieve_user_ID)

## 새 액세스 정책 작성
{: #create_policy}

특정 키에 대한 액세스 제어를 사용하기 위해 다음 명령을 실행하여 {{site.data.keyword.iamshort}}에 요청을 전송할 수 있습니다. 각각의 액세스 정책마다 명령을 반복 실행하십시오.

```cURL
curl -X POST \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "roles": [
    {
      "id": "crn:v1:bluemix:public:iam::::role:<IAM_role>"
    }
  ],
  "resources": [
    {
      "serviceName": "IBM Key Protect",
      "accountId": "<account_ID>",
      "region": "us-south",
      "serviceInstance": "<instance_ID>",
      "resourceType": "key",
      "resource": "<key_ID>"
    }
  ]
}'
```
{: codeblock}

지정된 Cloud Foundry 조직 및 영역 내 키에 대한 액세스를 관리해야 하는 경우, `serviceInstance`를 `organizationId` 및 `spaceId`로 바꾸십시오. 자세히 보려면 [액세스 관리 API 참조 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://iampap.ng.bluemix.net/v1/docs/#!/Access_Policies/){: new_window}를 참조하십시오.
{: tip}

`<user_ID>`, `<Admin_IAM_token>`, `<IAM_role>`, `<account_ID>`, `<instance_ID>` 및 `<key_ID>`을 적절한 값으로 대체하십시오.

**선택사항:** 정책 작성이 완료되었는지 확인하십시오.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}


## 액세스 정책 업데이트
{: #update_policy}

검색된 정책 ID를 사용하여 사용자에 대한 기존 정책을 수정할 수 있습니다. 다음 명령을 실행하여 {{site.data.keyword.iamshort}}에 요청을 전송하십시오.

```cURL
curl -X PUT \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies/<policy_ID> \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'If-Match': <ETag_ID> \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
  "roles": [
    {
      "id": "crn:v1:bluemix:public:iam::::role:<IAM_role>"
    }
  ],
  "resources": [
    {
      "serviceName": "IBM Key Protect",
      "accountId": "<account_ID>",
      "region": "us-south",
      "serviceInstance": "<instance_ID>",
      "resourceType": "key",
      "resource": "<key_ID>"
    }
  ]
}'
```
{: codeblock}

`<user_ID>`, `<Admin_IAM_token>`, `<IAM_role>`, `<account_ID>`, `<instance_ID>` 및 `<key_ID>`를 적절한 값으로 대체하십시오.

**선택사항:** 정책 업데이트가 완료되었는지 확인하십시오.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}

## 액세스 정책 삭제
{: #delete_policy}

검색된 정책 ID를 사용하여 사용자에 대한 기존 정책을 삭제할 수 있습니다. 다음 명령을 실행하여 {{site.data.keyword.iamshort}}에 요청을 전송하십시오.

```cURL
curl -X DELETE \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies/<policy_ID> \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}

`<account_ID>`, `<user_ID>`, `<policy_ID>` 및 `<Admin_IAM_token>`을 적절한 값으로 대체하십시오.

**선택사항:** 정책 삭제가 완료되었는지 확인하십시오.

```cURL
curl -X GET \
  https://iam.bluemix.net/acms/v1/scopes/a/<account_ID>/users/<user_ID>/policies \
  -H 'Authorization: Bearer <Admin_IAM_token>' \
  -H 'Accept: application/json' \
```
{: codeblock}

## 계정 ID 검색
{: #retrieve_account_id}

1. {{site.data.keyword.cloud_notm}} (bx) CLI에 로그인하십시오.
    ```sh
    bx login [--sso]
    ```
    {: codeblock}

    **참고**: `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

    결과에 계정에 대한 식별 정보가 표시됩니다.

    ```sh
    Authenticating...
    OK

    Select an account (or press enter to skip):

    1. sample-account (b6hnh3560ehqjkf89s4ba06i367801e)
    Enter a number> 1
    Targeted account sample-acount (b6hnh3560ehqjkf89s4ba06i367801e)

    API endpoint:   https://api.ng.bluemix.net (API version: 2.75.0)
    Region:         us-south
    User:           admin
    Account:        sample-account (b6hnh3560ehqjkf89s4ba06i367801e)
    ```
    {: screen}
2. 계정 ID에 대한 값을 복사하십시오.

## 사용자 ID 검색
{: #retrieve_user_id}

1. [자체 IAM 토큰을 제공하도록 사용자에게 요청](/docs/services/keymgmt/keyprotect_authentication.html#retrieve_token)하십시오.
    IAM 토큰 구조는 다음과 같습니다.

    ```sh
    IAM token: Bearer <value>.<value>.<value>
    ```
    {: screen}

2. 중간 값을 복사하고 다음 명령을 실행하십시오.
    ```sh
    echo -n "<value>" | base64 --decode
    ```
    {: codeblock}

    결과에 다음과 유사한 JSON 오브젝트가 표시됩니다.
   ```json
   {
        "iam_id":"...",
        "id":"...",
        "realmid":"...",
        "identifier":"...",
        "given_name":"...",
        "family_name":"...",
        "name":"...",
        "email":"...",
        "sub":"...",
        "account":{
            "bss":"..."},
            "iat":...,
            "exp":...,
            "iss":"...",
            "grant_type":"...",
            "scope":"...",
            "client_id":"..."
        }
   }
   ```
   {: screen}

4. `id` 특성의 값을 복사하십시오.
