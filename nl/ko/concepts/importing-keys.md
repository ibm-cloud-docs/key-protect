---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# 암호화 키를 클라우드로 가져오기
{: #importing-keys}

암호화 키에는 키 식별에 도움이 되는 메타데이터 및 데이터 암호화와 복호화에 사용되는 _키 자료_와 같은 정보의 서브세트가 포함됩니다.
{: shortdesc}

사용자가 {{site.data.keyword.keymanagementserviceshort}}를 사용하여 키를 작성하는 경우, 서비스는 사용자 대신 클라우드 기반 HSM(Hardware Security Module)을 바탕으로 하는 암호화 키 자료를 생성합니다. 하지만 비즈니스 요구사항에 따라 내부 솔루션에서 키 자료를 생성한 다음, 키를 {{site.data.keyword.keymanagementserviceshort}}로 가져와 온프레미스 키 관리 인프라를 클라우드로 확장해야 합니다.

<table>
  <th>이점</th>
  <th>설명</th>
  <tr>
    <td>BYOK(Bring Your Own Keys) </td>
    <td>온프레미스 HSM(Hardware Security Module)에서 강력한 키를 생성하여 키 관리 방식을 완전히 제어하고 강화할 수 있습니다. 내부 키 관리 인프라에서 대칭 키를 내보내도록 선택한 경우 {{site.data.keyword.keymanagementserviceshort}}를 사용하여 대칭 키를 안전하게 클라우드로 가져올 수 있습니다.</td>
  </tr>
  <tr>
    <td>루트 키 자료의 보안 가져오기</td>
    <td>키를 클라우드로 내보낼 때 이동하는 동안 키 자료가 보호되기를 원합니다. 루트 키 자료를 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스로 안전하게 가져오기 위해 <a href="#transport-keys">전송 키를 사용</a>하여 중간자 공격에 대비하십시오.</td>
  </tr>
  <caption style="caption-side:bottom;">표 1. 키 자료 가져오기의 이점에 대한 설명</caption>
</table>


## 키 자료를 가져오기 위한 계획
{: #plan-ahead}

루트 키 자료를 서비스로 가져올 준비가 되면 다음 고려사항에 주의하십시오.

<dl>
  <dt>키 자료 작성을 위한 옵션 검토</dt>
    <dd>보안 요구에 따라 256비트 대칭 암호화 키를 작성하는 옵션을 탐색하십시오. 예를 들어, FIPS로 유효성 검증된 온프레미스 HSM(Hardware Security Module)에서 지원하는 내부 키 관리 시스템을 사용하여, 키를 클라우드로 가져오기 전에 키 자료를 생성할 수 있습니다. 개념 증명을 빌드하는 경우 <a href="https://www.openssl.org/" target="_blank">OpenSSL</a>과 같은 암호 툴킷을 사용하여 테스트를 위해 {{site.data.keyword.keymanagementserviceshort}}로 가져올 수 있는 키 자료를 생성할 수도 있습니다.</dd>
  <dt>키 자료를 {{site.data.keyword.keymanagementserviceshort}}로 가져오기 위한 옵션을 선택하십시오.</dt>
    <dd>환경 또는 워크로드에 필요한 보안 레벨을 기반으로 루트 키를 가져오기 위한 옵션을 두 가지 옵션 중에서 선택하십시오. 기본적으로 {{site.data.keyword.keymanagementserviceshort}}는 TLS(Transport Layer Security) 1.2 프로토콜을 사용하여 이동 중인 키 자료를 암호화합니다. 개념 증명을 빌드하거나 처음으로 서비스를 시도하는 경우 이 기본 옵션을 사용하여 루트 키 자료를 {{site.data.keyword.keymanagementserviceshort}}로 가져올 수 있습니다. 워크로드에 TLS를 능가하는 보안 메커니즘이 필요한 경우에도 <a href="#transport-keys">전송 키를 사용</a>하여 루트 키 자료를 암호화하고 서비스로 가져올 수 있습니다.</dd>
  <dt>키 자료를 암호화하기 위한 계획</dt>
    <dd>전송 키를 사용하여 키 자료를 암호화하도록 선택한 경우 키 자료에 대한 RSA 암호화를 실행하기 위한 방법을 결정하십시오. <a href="https://tools.ietf.org/html/rfc3447" target="_blank">PKCS #1 v2.1 standard for RSA encryption</a>에 지정된 대로 <code>RSAES_OAEP_SHA_256</code> 암호화 스킴을 사용해야 합니다. 내부 키 관리 시스템 또는 온프레미스 HSM의 기능을 검토하여 옵션을 결정하십시오.</dd>
  <dt>가져온 키 자료의 라이프사이클 관리</dt>
    <dd>키 자료를 서비스로 가져온 후에는 키의 전체 라이프사이클을 관리해야 합니다. {{site.data.keyword.keymanagementserviceshort}} API를 사용하여, 키를 서비스에 업로드하도록 결정할 때 키의 만기 날짜를 설정할 수 있습니다. 하지만 <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">가져온 루트 키를 순환</a>하려면 새 키 자료를 생성하고 제공하여 기존 키를 폐기하고 바꿔야 합니다. </dd>
</dl>

## 전송 키 사용
{: #transport-keys}

전송 키는 현재 베타 기능입니다. 베타 기능은 언제든지 변경할 수 있으며 향후 업데이트는 최신 버전과 호환되지 않는 변경사항을 도입할 수 있습니다.
{: important}

키 자료를 {{site.data.keyword.keymanagementserviceshort}}로 가져오기 전에 암호화하려면 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 서비스 인스턴스의 전송 암호화 키를 작성할 수 있습니다. 

전송 키는 서비스 인스턴스로 루트 키 자료의 보안 가져오기를 사용하도록 설정하는 {{site.data.keyword.keymanagementserviceshort}}의 리소스 유형입니다. 전송 키로 온프레미스에서 키 자료를 암호화하여, 지정하는 정책에 따라 {{site.data.keyword.keymanagementserviceshort}}로 이동 중인 루트 키를 보호합니다. 예를 들어, 시간 및 사용량에 따라 키 사용을 제한하는 정책을 전송 키에 설정할 수 있습니다.

### 작동 방식
{: #how-transport-keys-work}

서비스 인스턴스의 [전송 키를 작성](/docs/services/key-protect?topic=key-protect-create-transport-keys)할 때 {{site.data.keyword.keymanagementserviceshort}}는 4096비트 RSA 키를 생성합니다. 서비스는 개인 키를 암호화한 다음 루트 키 자료를 암호화하고 가져오기 위해 사용할 수 있는 공개 키와 가져오기 토큰을 리턴합니다. 

{{site.data.keyword.keymanagementserviceshort}}로 [루트 키를 가져올](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-api) 준비가 되면 공개 키의 무결성을 확인하는 데 사용되는 가져오기 토큰과 암호화된 루트 키 자료를 제공합니다. 요청을 완료하기 위해 {{site.data.keyword.keymanagementserviceshort}}가 서비스 인스턴스와 연관된 개인 키를 사용하여 암호화된 루트 키 자료를 복호화합니다. 이 프로세스에서는 {{site.data.keyword.keymanagementserviceshort}}에서 생성한 전송 키만 서비스에 가져와 저장한 키 자료를 복호화할 수 있습니다.

서비스 인스턴스마다 하나의 전송 키만 작성할 수 있습니다. 전송 키의 검색 한계에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조](https://{DomainName}/apidocs/key-protect){: new_window}하십시오.
{: note} 

### API 메소드
{: #secure-import-api-methods}

{{site.data.keyword.keymanagementserviceshort}} API가 뒤에서 전송 키 작성 프로세스를 유도합니다.  

다음 표에는 로커(locker)를 설정하고 서비스 인스턴스의 전송 키를 작성하는 API 메소드가 나열되어 있습니다.

<table>
  <tr>
    <th>메소드</th>
    <th>설명</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">전송 키 작성</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">전송 키 메타데이터 검색</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">전송 키 및 가져오기 토큰 검색</a></td>
  </tr>
  <caption style="caption-side:bottom;">표 2. {{site.data.keyword.keymanagementserviceshort}} API 메소드에 대한 설명</caption>
</table>

{{site.data.keyword.keymanagementserviceshort}}에서 프로그래밍 방식으로 키를 관리하는 방법에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 확인하십시오.

## 다음에 수행할 작업

- 서비스 인스턴스의 전송 키를 작성하는 방법에 대해 알아보려면 [전송 키 작성](/docs/services/key-protect?topic=key-protect-create-transport-keys)을 참조하십시오.
- 서비스로 키 가져오기에 대해 자세히 알아보려면 [루트 키 가져오기](/docs/services/key-protect?topic=key-protect-import-root-keys)를 참조하십시오. 
