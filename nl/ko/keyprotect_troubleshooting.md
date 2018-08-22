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

# 문제점 해결

{{site.data.keyword.keymanagementservicefull}} 사용과 관련된 일반적인 문제점 해결 질문에 대한 답변은 다음과 같습니다.
{: shortdesc}

## 키를 작성하거나 삭제할 수 없음
{: #unabletomanagekeys}

{{site.data.keyword.keymanagementserviceshort}} 조치를 수행할 수 없는 경우 사용자에게 올바른 권한이 있는지 확인하십시오.

### 증상

{{site.data.keyword.keymanagementserviceshort}} 사용자 인터페이스에 액세스할 때, 키를 추가하거나 삭제하는 옵션을 볼 수 없습니다.

### 문제점 해결

적용 가능한 영역에서 사용자에게 올바른 역할이 지정되었는지 관리자에게 확인하십시오. 역할에 대한 자세한 정보는 [역할 및 권한](/docs/services/keymgmt/keyprotect_manage_access.html#roles)을 참조하십시오.

## 베타에서 GA(General Assembly)로 변환
{: #ts_planconversion}

베타 사용자가 {{site.data.keyword.keymanagementserviceshort}} 서비스를 계속 사용하려면 표준 가격 책정 모델로 변경해야 합니다.

### 증상

베타 사용자인 경우, {{site.data.keyword.keymanagementserviceshort}}에서 계속해서 키를 저장하려면 가격 책정 플랜을 계층 가격 책정 모델로 변경해야 합니다.

### 문제점 해결

{{site.data.keyword.keymanagementserviceshort}} 서비스에는 라이트 및 누진 계층의 두 가지 가격 책정 모델이 있습니다. 관리자로서 누진 계층 모델이 선택되었는지 확인하십시오. 계층 모델로 마이그레이션하지 않으려는 경우에는 사용 중단 전에 모든 키와 서비스 인스턴스가 제거되었는지 확인하십시오. [자세한 정보는 {{site.data.keyword.keymanagementserviceshort}} 일반 어셈블리 공지사항을 참조하십시오. ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2016/12/dallas-key-protect-ga/){: new_window}

가격 책정 플랜을 변경하려면 다음을 수행하십시오.

1. {{site.data.keyword.keymanagementserviceshort}} 사용자 인터페이스로 이동하고 **플랜** 탭을 선택하십시오.
2. **누진 계층 가격 책정**을 선택하십시오.
    월별로 사용되는 활성 키에 대한 비용 명세가 표에 나타납니다. 계층 모델은 계정 레벨에서 월별 활성 키의 수를 기반으로 가격 책정을 산출합니다.
3. 플랜 변경사항을 확정하려면 **저장**을 클릭하십시오.

## {{site.data.keyword.keymanagementserviceshort}}에 대한 도움 및 지원 받기
{: #ts_gettinghelp}

{{site.data.keyword.keymanagementservicefull_notm}}의 사용 시에 문제점이나 질문이 있는 경우 정보를 검색하거나 포럼을 통해 질문하여 도움을 받을 수 있습니다. 또한 지원 티켓을 열 수도 있습니다.

포럼을 사용하여 질문하는 경우에는 {{site.data.keyword.cloud_notm}} 개발 팀이 볼 수 있도록 질문에 태그를 지정하십시오.

- {{site.data.keyword.keymanagementserviceshort}}에 대한 기술적 질문이 있는 경우 [스택 오버플로우 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://stackoverflow.com/search?q=key-protect+ibm-bluemix){: new_window}에 질문을 게시하고 질문에 "ibm-bluemix" 및 "key-protect" 태그를 지정하십시오.
- 서비스 및 시작하기 지시사항에 대한 질문이 있는 경우 [IBM developerWorks dW Answers ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} 포럼을 활용하십시오. "bluemix" 및 "key-protect" 태그를 지정하십시오.

포럼 활용에 대한 세부사항은 [도움 받기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}를 참조하십시오.

{{site.data.keyword.IBM_notm}} 지원 티켓 열기 또는 지원 레벨과 티켓 심각도에 대한 정보는 [지원 문의 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}를 참조하십시오.
