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

# 서비스 프로비저닝
{: #provision}

{{site.data.keyword.cloud_notm}} 콘솔 또는 {{site.data.keyword.cloud_notm}} CLI를 사용하여 {{site.data.keyword.keymanagementservicefull}}의 인스턴스를 작성할 수 있습니다.
{: shortdesc}

## {{site.data.keyword.cloud_notm}} 콘솔에서 프로비저닝
{: #gui}

{{site.data.keyword.cloud_notm}} 콘솔에서 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝하려면 다음 단계를 완료하십시오.

1. [{{site.data.keyword.cloud_notm}} 계정 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}){: new_window}에 로그인하십시오.
2. **카탈로그**를 클릭하여 {{site.data.keyword.cloud_notm}}에서 사용 가능한 서비스의 목록을 보십시오.
3. 모든 카테고리 탐색 분할창에서 **보안 및 ID** 카테고리를 클릭하십시오.
4. 서비스 목록에서 **{{site.data.keyword.keymanagementserviceshort}}** 타일을 클릭하십시오.
5. 서비스 플랜을 선택하고 **작성**을 클릭하여 계정, 지역 및 로그인된 리소스 그룹에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝하십시오.

## {{site.data.keyword.cloud_notm}} CLI에서 프로비저닝
{: #cli}

{{site.data.keyword.cloud_notm}} CLI를 사용하여 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝할 수 있습니다. 

### 계정 내 서비스 인스턴스 작성
{: #provision-acct-lvl}

[{{site.data.keyword.iamlong}} 역할](/docs/iam/users_roles.html#iamusermanrol)로 암호화 키에 대한 액세스를 단순화하기 위해 조직 및 영역을 지정할 필요 없이 계정 내에서 {{site.data.keyword.keymanagementserviceshort}} 서비스의 인스턴스를 하나 이상 작성할 수 있습니다. 

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window}를 통해 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **참고:** 로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

2. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 작성할 계정, 지역 및 리소스 그룹을 선택하십시오.

    다음 명령을 사용하여 대상 지역과 리소스 그룹을 설정할 수 있습니다.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. 해당 계정 및 리소스 그룹 내에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝하십시오.

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    `<instance_name>`을 서비스 인스턴스의 고유 별명으로 바꾸십시오.

4. 선택사항: 서비스 인스턴스가 작성되었는지 확인하십시오.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

### 조직 및 영역 내에서 서비스 인스턴스 작성
{: #provision-space-lvl}

[Cloud Foundry 역할](/docs/iam/cfaccess.html)로 암호화 키에 대한 액세스를 관리하기 위해 지정된 조직 및 영역 내에서 {{site.data.keyword.keymanagementserviceshort}} 서비스의 인스턴스를 작성할 수 있습니다.  

1. [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/index.html#overview){: new_window}를 통해 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **참고:** 로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

2. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 작성할 계정, 지역, 조직 및 영역을 선택하십시오.

    다음 명령을 사용하여 대상 지역, 조직 및 영역을 설정할 수 있습니다.

    ```sh
    ibmcloud target -r <region> -o <organization_name> -s <space_name>
    ```
    {: pre}

3. 해당 계정, 지역, 조직 및 영역 내에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝하십시오.

    ```sh
    ibmcloud service create kms tiered-pricing <instance_name>
    ```
    {: pre}

    `<instance_name>`을 서비스 인스턴스의 고유 별명으로 바꾸십시오.

4. 선택사항: 서비스 인스턴스가 작성되었는지 확인하십시오.

    ```sh
    ibmcloud service list
    ```
    {: pre}


### 다음에 수행할 작업

- 데이터를 암호화하고 복호화하기 위해 {{site.data.keyword.keymanagementserviceshort}}에 저장된 키가 작동하는 방식에 대한 예를 보려면 [GitHub에서 샘플 앱 확인![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}을 수행하십시오.
- 프로그래밍 방식의 키 관리에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 확인하십시오.
