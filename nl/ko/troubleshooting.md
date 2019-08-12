---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# 문제점 해결
{: #troubleshooting}

{{site.data.keyword.keymanagementservicefull}} 사용과 관련된 일반 문제점에는 API와 상호작용할 때 올바른 헤더 또는 인증 정보를 제공하는 것이 포함될 수 있습니다. 많은 경우에 다음과 같은 몇 가지 쉬운 단계를 통해 이러한 문제점에서 복구할 수 있습니다.
{: shortdesc}

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

[상태 페이지](https://{DomainName}/status?tags=platform,runtimes,services){: external}로 이동하여 {{site.data.keyword.cloud_notm}}가 사용 가능한지 여부를 확인할 수 있습니다.

포럼을 검토하여 다른 사용자에게 동일한 문제점이 발생했는지 확인할 수 있습니다. 포럼을 사용하여 질문하는 경우에는 {{site.data.keyword.cloud_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

- {{site.data.keyword.keymanagementserviceshort}}에 대한 기술 질문이 있는 경우 [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external}에 질문을 게시하고 질문에 "ibm-cloud" 및 "key-protect" 태그를 지정하십시오.
- 서비스 및 시작하기 지시사항에 대한 질문은 [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external} 포럼을 사용하십시오. "ibm-cloud" 및 "key-protect" 태그를 포함하십시오.

포럼 사용에 대한 세부사항은 [지원 받기](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external}를 참조하십시오.

{{site.data.keyword.IBM_notm}} 지원 티켓 열기 또는 지원 레벨과 티켓 심각도에 대한 자세한 정보는 [지원 문의](/docs/get-support?topic=get-support-getting-customer-support){: external}를 참조하십시오.
