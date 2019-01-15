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

# CLI 설정
{: #set-up-cli}

{{site.data.keyword.keymanagementservicelong_notm}} CLI 플러그인을 사용하여 암호화 키를 작성하고, 가져오고, 관리할 수 있습니다. 

CLI 사용에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} CLI 참조 문서](/docs/services/key-protect/cli-reference.html)를 확인하십시오.
{: tip}

## {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인 설치
{: #install-cli}

{{site.data.keyword.keymanagementserviceshort}} CLI 플러그인을 설정하려면 [{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#overview){: new_window}을 설치하십시오. 

CLI를 설치하려면 다음을 수행하십시오.

1. [{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#overview){: new_window}를 설치하십시오.

    CLI를 설치한 후 `ibmcloud` 명령을 실행하여 클라우드 서비스와 상호작용할 수 있습니다.

2. {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **참고:** 로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

3. 암호화 키 관리를 시작하려면 {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인을 설치하십시오.

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. 선택사항: 플러그인이 설치되었는지 확인하십시오.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인 업데이트
{: #update-cli}

새 기능을 사용하기 위해 CLI를 주기적으로 업데이트하려고 할 수 있습니다.

CLI를 업데이트하려면 다음을 수행하십시오.

1. [{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#overview){: new_window}를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **참고:** 로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

2. 플러그인 저장소에서 업데이트를 설치하십시오.

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. 선택사항: 플러그인이 업데이트되었는지 확인하십시오.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인 설치 제거
{: #uninstall-cli}

1. [{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli/index.html#overview){: new_window}를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **참고:** 로그인에 실패하면 `ibmcloud login --sso` 명령을 실행하여 다시 시도하십시오. `--sso` 매개변수는 연합 ID로 로그인할 때 필요합니다. 이 옵션을 사용하는 경우에는 CLI 출력에 나열된 링크로 이동하여 일회성 패스코드를 생성하십시오.

2. 플러그인 저장소에서 업데이트를 설치하십시오.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. 선택사항: 플러그인이 설치 제거되었는지 확인하십시오.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}
