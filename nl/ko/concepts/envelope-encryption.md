---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data-at-rest encryption, envelope encryption, root key, data encryption key, protect data encryption key, encrypt data encryption key, wrap data encryption key, unwrap data encryption key

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

# 엔벌로프 암호화로 데이터 보호
{: #envelope-encryption}

엔벨로프 암호화는 데이터 암호화 키(DEK)로 데이터를 암호화한 다음 완전히 관리할 수 있는 루트 키로 DEK를 암호화하는 것입니다. 
{: shortdesc}

{{site.data.keyword.keymanagementservicefull}}는 고급 암호화를 사용하여 저장된 데이터를 보호하고 여러 이점을 제공합니다.

|이점 |설명 |
| --- | --- |
|고객 관리 암호화 키 |서비스를 사용하면 루트 키를 프로비저닝하여 클라우드에서 암호화된 데이터의 보안을 보호할 수 있습니다. 루트 키는 마스터 키-랩핑 키의 역할을 하며, 이 키를 사용하면 {{site.data.keyword.cloud_notm}} 데이터 서비스에 프로비저닝된 데이터 암호화 키(DEK)를 관리하고 보호할 수 있습니다. 기존 루트 키를 가져올지 아니면 {{site.data.keyword.keymanagementserviceshort}}에서 루트 키를 생성하도록 할지를 결정합니다. |
|비밀 및 무결성 보호 | {{site.data.keyword.keymanagementserviceshort}}는 GCM(Galois/Counter Mode)에서 고급 암호화 표준(AES) 알고리즘을 사용하여 키를 보호합니다. 사용자가 서비스에서 키를 작성할 때, {{site.data.keyword.keymanagementserviceshort}}가 {{site.data.keyword.cloud_notm}} HSM(Hardware Security Module)의 신뢰 경계 내에서 이 키를 생성하므로 암호화 키에 사용자만 액세스할 수 있습니다. |
|데이터의 암호 파쇄  |조직이 보안 문제를 발견하거나 앱이 데이터 세트를 더 이상 필요로 하지 않는 경우 클라우드에서 데이터를 영구적으로 파쇄하도록 선택할 수 있습니다. 다른 DEK를 보호하는 루트 키를 삭제하면 키의 연관된 데이터에 더 이상 액세스할 수 없거나 이를 복호화할 수 없습니다. |
|위임된 사용자 액세스 제어 |{{site.data.keyword.keymanagementserviceshort}}는 중앙화된 액세스 제어 시스템을 지원하여 키에 대한 세부적 액세스를 사용합니다. [IAM 사용자 역할 및 고급 권한을 지정하여](/docs/services/key-protect?topic=key-protect-manage-access#roles) 보안 관리자가 서비스의 어떤 루트 키에 어떤 사용자가 액세스할 수 있는지를 결정합니다. |
{: caption="표 1. 고객 관리 암호화의 이점에 대한 설명" caption-side="top"}

## 작동 방식
{: #overview}

엔벨로프 암호화는 여러 암호화 알고리즘의 장점을 결합하여 클라우드의 민감한 데이터를 보호합니다. 이 암호화가 작동하려면 완전히 관리할 수 있는 루트 키를 사용하여 고급 암호화를 통해 하나 이상의 데이터 암호화 키(DEK)를 랩핑해야 합니다. 이 키 랩핑 프로세스는 무단 액세스 또는 노출로부터 저장된 데이터를 보호하는 랩핑된 DEK를 작성합니다. DEK를 랩핑 해제하면 동일한 루트 키를 사용하여 엔벨로프 암호화 프로세스를 되돌릴 수 있으며 그 결과 데이터가 복호화되고 인증됩니다.
 
다음 다이어그램은 키 랩핑 기능의 컨텍스트 보기를 표시합니다.
![다이어그램은 엔벨로프 암호화의 컨텍스트 보기를 표시합니다.](../images/envelope-encryption_min.svg)

엔벨로프 암호화는 NIST Special Publication 800-57, Recommendation for Key Management에서 간단하게 다뤄집니다. 자세한 내용은 다음을 참조하십시오. [NIST SP 800-57 Pt. 1 Rev. 4.](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}

## 키 유형
{: #key-types}

서비스는 데이터의 고급 암호화 및 관리를 위해 두 가지 키 유형(루트 키와 표준 키)을 지원합니다.

<dl>
  <dt>루트 키</dt>
    <dd>루트 키는 {{site.data.keyword.keymanagementserviceshort}}의 기본 리소스입니다. 또한 데이터 서비스에 저장된 기타 키를 랩핑(암호화)하고 랩핑 해제(복호화)하기 위한 신뢰 루트로 사용되는 대칭 키-랩핑 키입니다. {{site.data.keyword.keymanagementserviceshort}}를 사용하면 루트 키의 라이프사이클을 작성하고 저장하고 관리하여 클라우드에 저장된 다른 키를 완전히 제어할 수 있습니다. 표준 키와 달리 루트 키는 {{site.data.keyword.keymanagementserviceshort}} 서비스 외에 사용될 수 없습니다.</dd>
  <dt>표준 키</dt>
    <dd>표준 키는 비밀번호 또는 암호화 키와 같은 시크릿을 지속할 수 있는 한 가지 방법입니다. {{site.data.keyword.keymanagementserviceshort}}를 사용하여 표준 키를 저장하는 경우 시크릿에 맞는 하드웨어 보안 모듈(HSM) 스토리지, 리소스에 대한 세분화된 액세스 제어(<a href="/docs/services/key-protect?topic=key-protect-manage-access" target="_blank">{{site.data.keyword.iamshort}}(IAM)</a> 사용) 및 서비스에 대한 API 호출을 감사할 수 있는 기능(<a href="/docs/services/key-protect?topic=key-protect-at-events" target="_blank">{{site.data.keyword.cloudaccesstrailshort}}</a> 사용)을 사용으로 설정합니다.</dd>
</dl>

{{site.data.keyword.keymanagementserviceshort}}에서 키를 작성한 후 시스템은 서비스에 대한 API 호출을 작성하는 데 사용할 수 있는 ID 값을 리턴합니다. {{site.data.keyword.keymanagementserviceshort}} GUI 또는 [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect)를 사용하여 키에 대한 ID 값을 검색할 수 있습니다. 

## 키 랩핑
{: #wrapping}

루트 키를 사용하면 클라우드에 저장된 데이터 암호화 키(DEK)를 그룹화하고 관리하고 보호할 수 있습니다. 완전히 관리할 수 있는 {{site.data.keyword.keymanagementserviceshort}}의 루트 키를 지정하여 고급 암호화로 하나 이상의 DEK를 랩핑할 수 있습니다. 

{{site.data.keyword.keymanagementserviceshort}}에서 루트 키를 지정한 후 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 서비스로 키 랩핑 요청을 전송할 수 있습니다. 키 랩핑 오퍼레이션은 DEK를 위한 비밀과 무결성 보호를 제공합니다. 다음 다이어그램은 조치의 키 랩핑 프로세스를 표시합니다.
![다이어그램은 조치의 키 랩핑 프로세스를 표시합니다.](../images/wrapping-keys_min.svg)

다음 표에는 키 랩핑 오퍼레이션을 수행하는 데 필요한 입력이 설명되어 있습니다.

|입력 |설명 |
| --- | --- |
|루트 키 ID |랩핑에 사용할 루트 키의 ID 값입니다. 루트 키를 서비스로 가져오거나 HSM의 {{site.data.keyword.keymanagementserviceshort}}에서 이 키를 생성할 수 있습니다. 랩핑 요청이 성공하려면 랩핑에 사용되는 루트 키가 128, 192 또는 256비트여야 합니다. |
|일반 텍스트 | 선택사항: 데이터 암호화에 사용할 데이터 암호화 키(DEK)입니다. 이 값은 base64로 인코딩되어야 합니다. 새 DEK를 생성하려면 `plaintext` 특성을 생략할 수 있습니다. 키 보호는 HSM을 바탕으로 하는 랜덤 일반 텍스트(32바이트)를 생성한 후 해당 값을 랩핑합니다. |
|추가 인증 데이터(AAD) |선택사항: 키 컨텐츠의 무결성을 검사하는 문자열의 배열입니다. 각 문자열은 최대 255자입니다. 랩핑 요청 중에 AAD를 제공하면 후속 랩핑 해제 요청 중에 동일한 AAD를 지정해야 합니다. |
{: caption="표 2. {{site.data.keyword.keymanagementserviceshort}}에서 키 랩핑에 필요한 입력" caption-side="top"}

암호화할 일반 텍스트를 지정하지 않고 랩핑 요청을 보내는 경우 AES-GCM 암호화 알고리즘이 일반 텍스트를 생성하고 암호문이라고 하는 이해할 수 없는 양식의 데이터로 변환합니다. 이 프로세스는 새 키 자료와 함께 256비트 DEK를 출력합니다. 그런 다음 시스템은 AES 키-랩핑 알고리즘을 사용하며, 이 알고리즘을 통해 지정된 루트 키로 DEK와 해당 키 자료를 랩핑합니다. 성공한 랩핑 오퍼레이션은 {{site.data.keyword.cloud_notm}} 앱 또는 서비스에 저장할 수 있는 랩핑된 base64로 인코딩된 DEK를 리턴합니다. 

## 키 랩핑 해제
{: #unwrapping}

데이터 암호화 키(DEK)를 랩핑 해제하면 키 내에서 컨텐츠가 복호화되고 인증되며, 원래 키 자료가 데이터 서비스에 리턴됩니다. 

비즈니스 애플리케이션이 랩핑된 DEK의 컨텐츠에 액세스해야 하는 경우 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 서비스로 랩핑 해제 요청을 보낼 수 있습니다. DEK를 랩핑 해제하려면 초기 랩핑 요청 중에 리턴된 `ciphertext` 값 및 루트 키의 ID 값을 지정합니다. 또한 랩핑 해제 요청을 완료하려면 추가 인증 데이터(AAD)를 제공하여 키 컨텐츠의 무결성을 검사해야 합니다.

다음 다이어그램은 조치의 키 랩핑 해제를 보여줍니다.
![다이어그램은 데이터 랩핑 해제 방식을 보여줍니다.](../images/unwrapping-keys_min.svg)

랩핑 해제 요청을 보내면 시스템이 동일한 AES 알고리즘을 사용하여 키 랩핑 프로세스를 되돌립니다. 성공한 랩핑 해제 오퍼레이션은 base64로 인코딩된 `plaintext` 값을 {{site.data.keyword.cloud_notm}} 저장 데이터 서비스로 리턴합니다.




