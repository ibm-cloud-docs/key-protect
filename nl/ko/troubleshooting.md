---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 문제점 해결
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} 사용과 관련된 일반 문제점에는 API와 상호작용할 때 올바른 헤더 또는 인증 정보를 제공하는 것이 포함될 수 있습니다. 많은 경우에 다음과 같은 몇 가지 쉬운 단계를 통해 이러한 문제점에서 복구할 수 있습니다.
{: shortdesc}

## 내 Cloud Foundry 서비스 인스턴스를 삭제할 수 없음
{: #unable-to-delete-service}

{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 삭제하려는 경우 예상대로 서비스를 삭제하는 데 실패합니다.

{{site.data.keyword.cloud_notm}} 대시보드에서 **Cloud Foundry 서비스**로 이동하고 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 선택합니다. ⋮ 아이콘을 클릭하여 서비스 오퍼링에 대한 옵션 목록을 열고 **서비스 삭제**를 클릭합니다.
{: tsSymptoms}

서비스를 삭제하는 데 실패하며, 다음 오류가 표시됩니다. 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

2017년 12월 15일에 {{site.data.keyword.keymanagementserviceshort}}가 Cloud Foundry 조직, 영역 및 역할 사용에서 IAM 및 리소스 그룹 사용으로 이동되었습니다. 이제 Cloud Foundry 조직 및 영역을 지정할 필요 없이 리소스 그룹 내에서 {{site.data.keyword.keymanagementserviceshort}} 서비스를 프로비저닝할 수 있습니다.
{: tsCauses}

이러한 변경사항은 이전 서비스 인스턴스에 대한 디프로비저닝 작동 방식에 영향을 미쳤습니다. 2017년 9월 28일 이전에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 작성한 경우 서비스 삭제가 예상대로 작동하지 않을 수 있습니다.

이전 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 삭제하려면 먼저 레거시 `https://ibm-key-protect.edge.bluemix.net` 엔드포인트 사용을 통해 기존 키를 삭제하여 {{site.data.keyword.keymanagementserviceshort}} 서비스와 상호작용해야 합니다.
{: tsResolve}

키 및 서비스 인스턴스를 삭제하려면 다음을 수행하십시오.

1. {{site.data.keyword.cloud_notm}} CLI를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **참고:** 로그인에 실패하면 `bx login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

2. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 포함된 {{site.data.keyword.cloud_notm}} 지역, 조직 및 영역을 선택하십시오.

    CLI 출력에 있는 조직 및 영역의 이름을 기록해 두십시오. 또한 `ibmcloud cf target`을 실행하여 Cloud Foundry 조직 및 영역을 대상으로 지정할 수도 있습니다.

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. {{site.data.keyword.cloud_notm}} 조직 및 영역 GUID를 검색하십시오.

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
`<organization_name>` 및 `<space_name>`을 조직 및 영역에 지정된 고유 별명으로 바꾸십시오.

4. 액세스 토큰을 검색하십시오.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. 다음 cURL 명령을 실행하여 서비스 인스턴스에 저장되는 키를 나열하십시오.

    ```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`, `<organization_GUID>` 및 `<space_GUID>`를 3 - 4단계에서 검색한 값으로 바꾸십시오. 

6. 서비스 인스턴스에 저장되는 각 키마다 ID 값을 복사하십시오.

7. 다음 cURL 명령을 실행하여 키와 해당 컨텐츠를 영구 삭제하십시오.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`, `<organization_GUID>`, `<space_GUID>` 및 `<key_ID>`를 3 - 5단계에서 검색한 값으로 바꾸십시오. 각 키마다 명령을 반복 실행하십시오.    

8. 키가 다음 cURL 명령을 실행하여 삭제되었는지 확인하십시오.

    ```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    `<access_token>`, `<organization_GUID>` 및 `<space_GUID>`를 3 - 4단계에서 검색한 값으로 바꾸십시오. 

9. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 삭제하십시오.

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. 선택사항: {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 {{site.data.keyword.cloud_notm}} 대시보드로 이동하여 삭제되었는지 확인하십시오.

    다음 명령을 실행하여 대상으로 지정된 영역에 사용 가능한 Cloud Foundry 서비스를 나열할 수도 있습니다.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## 사용자 인터페이스에 액세스할 수 없음
{: #unable-to-access-ui}

{{site.data.keyword.keymanagementserviceshort}} 사용자 인터페이스에 액세스할 때 UI가 예상대로 로드되지 않습니다.

{{site.data.keyword.cloud_notm}} 대시보드에서 {{site.data.keyword.keymanagementserviceshort}} 서비스의 인스턴스를 선택합니다.
{: tsSymptoms}

다음 오류가 표시됩니다. 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

2017년 12월 15일에 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-envelope-encryption)와 같은 새로운 기능을 {{site.data.keyword.keymanagementserviceshort}} 서비스에 추가했습니다. 이제 Cloud Foundry 조직 및 영역을 지정할 필요 없이 리소스 그룹 내에서 {{site.data.keyword.keymanagementserviceshort}} 서비스를 프로비저닝할 수 있습니다.
{: tsCauses}

이러한 변경사항은 이전 서비스 인스턴스의 사용자 인터페이스에 영향을 미쳤습니다. 2017년 9월 28일 이전에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 작성한 경우 사용자 인터페이스가 예상대로 작동하지 않을 수 있습니다.

IBM에서 이 문제를 해결하기 위해 노력하고 있습니다. 임시 솔루션으로 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 계속 키를 관리할 수 있습니다.
{: tsResolve}

레거시 `https://ibm-key-protect.edge.bluemix.net` 엔드포인트를 사용하여 {{site.data.keyword.keymanagementserviceshort}} 서비스와 상호작용할 수 있습니다.

**예제 요청**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## 키를 작성하거나 삭제할 수 없음
{: #unable-to-create-keys}

{{site.data.keyword.keymanagementserviceshort}} 사용자 인터페이스에 액세스할 때 키를 추가하거나 삭제하는 옵션이 표시되지 않습니다.

{{site.data.keyword.cloud_notm}} 대시보드에서 {{site.data.keyword.keymanagementserviceshort}} 서비스의 인스턴스를 선택합니다.
{: tsSymptoms}

키 목록을 볼 수 있지만 키를 추가하거나 삭제하는 옵션이 표시되지 않습니다. 

{{site.data.keyword.keymanagementserviceshort}} 조치를 수행할 수 있는 올바른 권한이 없습니다.
{: tsCauses} 

적용 가능한 리소스 그룹 또는 서비스 인스턴스에서 사용자에게 올바른 역할이 지정되었는지 관리자에게 확인하십시오. 역할에 대한 자세한 정보는 [역할 및 권한](/docs/services/key-protect?topic=key-protect-manage-access#roles)을 참조하십시오.
{: tsResolve}

## 도움 및 지원 받기
{: #getting-help}

{{site.data.keyword.keymanagementserviceshort}} 사용 시에 문제점이나 질문이 있는 경우 {{site.data.keyword.cloud_notm}}를 확인하거나, 정보를 검색하거나 포럼을 통해 질문하여 도움을 받을 수 있습니다. 또한 지원 티켓을 열 수도 있습니다.
{: shortdesc}

[상태 페이지 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/status?tags=platform,runtimes,services)로 이동하여 {{site.data.keyword.cloud_notm}}가 사용 가능한지 여부를 확인할 수 있습니다.

포럼을 검토하여 다른 사용자에게 동일한 문제점이 발생했는지 확인할 수 있습니다. 포럼을 사용하여 질문하는 경우에는 {{site.data.keyword.cloud_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

- {{site.data.keyword.keymanagementserviceshort}}에 대한 기술적 질문이 있는 경우 [Stack Overflow ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window}에 질문을 게시하고 질문에 "ibm-cloud" 및 "key-protect" 태그를 지정하십시오.
- 서비스 및 시작하기 지시사항에 대한 질문이 있는 경우 [IBM developerWorks dW Answers ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/key-protect/){: new_window} 포럼을 활용하십시오. "ibm-cloud" 및 "key-protect" 태그를 포함하십시오.

포럼 사용에 대한 세부사항은 [지원 받기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window}를 참조하십시오.

{{site.data.keyword.IBM_notm}} 지원 티켓 열기 또는 지원 레벨과 티켓 심각도에 대한 자세한 정보는 [지원 문의 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}를 참조하십시오.
