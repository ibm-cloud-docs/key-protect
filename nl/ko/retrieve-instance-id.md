---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# 인스턴스 ID 검색
{: #retrieve-instance-ID}

서비스에 대한 API 요청에 고유 ID 또는 인스턴스 ID를 포함하여 오퍼레이션에 대해 개별 {{site.data.keyword.keymanagementservicelong}} 서비스 인스턴스를 대상으로 지정할 수 있습니다.
{: shortdesc}

## {{site.data.keyword.cloud_notm}} 콘솔에서 인스턴스ID 보기
{: #view-instance-ID}

{{site.data.keyword.cloud_notm}} 리소스 목록으로 이동하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스와 연관된 인스턴스 ID를 볼 수 있습니다.

1. [{{site.data.keyword.cloud_notm}} 콘솔에 로그인하십시오](https://{DomainName}){: external}.
2. **메뉴** &gt; **리소스 목록**으로 이동한 다음 **서비스**를 클릭하여 클라우드 서비스 목록을 찾아보십시오.
3. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 설명하는 테이블 행을 클릭하십시오.
4. 서비스 세부사항 보기에서 **GUID** 값을 복사하십시오.

    이 **GUID** 값은 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 고유하게 식별하는 인스턴스 ID를 나타냅니다.

## CLI를 사용하여 인스턴스 ID 검색
{: #retrieve-instance-ID-cli}

[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}를 사용하여 서비스 인스턴스에 대한 인스턴스 ID를 검색할 수도 있습니다.

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.
    {: note}

2. {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스가 포함된 계정, 지역 및 리소스 그룹을 선택하십시오.

3. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 고유하게 식별하는 클라우드 리소스 이름(CRN)을 검색하십시오. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    `<instance_name>`을 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정한 고유 별명으로 바꾸십시오. 다음의 잘려진 예는 CLI 출력을 표시합니다.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    _42454b3b-5b06-407b-a4b3-34d9ef323901_ 값은 인스턴스 ID의 예입니다.


## API를 사용하여 인스턴스 ID 검색
{: #retrieve-instance-ID-api}

애플리케이션을 빌드하고 연결하는 데 도움이 되도록 프로그래밍 방식으로 인스턴스 ID를 검색할 수 있습니다. [{{site.data.keyword.cloud_notm}} 리소스 제어기 API](https://{DomainName}/apidocs/resource-controller)를 호출한 후 JSON 출력을 `jq`로 보내 이 값을 추출할 수 있습니다. 

1. [{{site.data.keyword.cloud_notm}} IAM 액세스 토큰을 검색](/docs/services/key-protect?topic=key-protect-retrieve-access-token)하십시오.
2. [리소스 제어기 API](https://{DomainName}/apidocs/resource-controller)를 호출하여 인스턴스 ID를 검색하십시오.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    `<instance_name>`을 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정한 고유 별명으로 바꾸십시오. 다음 출력은 인스턴스 ID 예를 표시합니다.

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}
