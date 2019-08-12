---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# 새로운 기능
{: #releases}

{{site.data.keyword.keymanagementservicefull}}에 사용 가능한 새 기능을 최신 상태로 유지하십시오. 
{: shortdesc}

## 2019년 6월
{: #june-2019}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 {{site.data.keyword.at_full_notm}}에 대한 지원을 추가함
{: #added-at-logdna-support}
신규 기준일: 2019년 6월 22일

이제 {{site.data.keyword.at_full_notm}}를 사용하여 {{site.data.keyword.keymanagementserviceshort}} 서비스에 대한 API 호출을 모니터할 수 있습니다.  

{{site.data.keyword.keymanagementserviceshort}} 활동 모니터링에 대해 자세히 알아보려면 [활동 트래커 이벤트](/docs/services/key-protect?topic=key-protect-at-events)를 참조하십시오.

## 2019년 5월
{: #may-2019}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}에서 HSM을 FIPS 140-2 Level 3로 업그레이드
{: #upgraded-hsms}
신규 기준일: 2019년 5월 22일

{{site.data.keyword.keymanagementserviceshort}}에서는 이제 암호화 스토리지 및 오퍼레이션에 {{site.data.keyword.cloud_notm}} Hardware Security Module 7.0을 사용합니다. 사용자의 {{site.data.keyword.keymanagementserviceshort}} 키는 모든 지역에 대해 FIPS 140-2 Level 3과 호환되는 하드웨어에 저장됩니다.  

{{site.data.keyword.cloud_notm}} HSM 7.0의 기능 및 혜택에 대해 더 알아보려면 [제품 페이지](https://www.ibm.com/cloud/hardware-security-module){: external}를 확인하십시오.

### 지원 종료: Cloud Foundry 기반 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스
{: #legacy-service-eol}
신규 기준일: 2019년 5월 15일

Cloud Foundry 기반의 레거시 {{site.data.keyword.keymanagementserviceshort}} 서비스는 2019년 5월 15일부로 지원이 종료되었습니다. Cloud Foundry 관리 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스는 더 이상 지원되지 않으며, 레거시 서비스로의 업그레이드도 더 이상 제공되지 않습니다. 고객은 서비스의 최신 기능을 활용하기 위해 IAM에서 관리되는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 사용하도록 권장됩니다.

2017년 12월 15일 이후에 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 작성한 경우, 서비스 인스턴스는 IAM에서 관리되며 이 변경사항의 영향을 받지 않습니다. 추가 질문은 Terry Mosbaugh([mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com))에 문의하십시오.

{{site.data.keyword.cloud_notm}} 리소스 목록의 **Cloud Foundry Services** 섹션에서 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 제거해야 합니까? [지원 센터](https://{DomainName}/unifiedsupport/cases/add)에 방문하여 콘솔 보기에서 항목 제거 요청을 제출하십시오.
{: tip}

## 2019년 3월
{: #mar-2019}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 정책 기반 키 순환에 대한 지원을 추가함
{: #added-support-policy-key-rotation}
신규 기준일: 2019년 3월 22일

{{site.data.keyword.keymanagementserviceshort}}를 사용하여 루트 키에 대한 순환 정책을 연관시킬 수 있습니다.

자세한 정보는 [순환 정책 설정](/docs/services/key-protect?topic=key-protect-set-rotation-policy)을 참조하십시오. {{site.data.keyword.keymanagementserviceshort}}에서 키 순환 옵션에 대해 자세히 알아보려면 [키 순환 옵션 비교](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)를 참조하십시오.

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 전송 키에 대한 베타 지원을 추가함
신규 기준일: 2019년 3월 20일

{{site.data.keyword.keymanagementserviceshort}} 서비스의 전송 암호화 키를 작성하여 클라우드로 암호화 키 보안 가져오기를 사용하십시오.

자세한 정보는 [클라우드로 암호화 키 가져오기](/docs/services/key-protect?topic=key-protect-importing-keys)를 참조하십시오.

전송 키는 현재 베타 기능입니다. 베타 기능은 언제든지 변경할 수 있으며 향후 업데이트는 최신 버전과 호환되지 않는 변경사항을 도입할 수 있습니다.
{: important}

## 2019년 2월
{: #feb-2019}

### 변경됨: 레거시 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스
{: #changed-legacy-service-instances}

신규 기준일: 2019년 2월 13일

2017년 12월 15일 전에 프로비저닝된 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스는 Cloud Foundry를 기반으로 한 레거시 인프라에서 실행됩니다. 이 레거시 {{site.data.keyword.keymanagementserviceshort}} 서비스는 2019년 5월 15일에 종료됩니다.

**의미하는 내용**

이전 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 활성 프로덕션 키가 있는 경우, 암호화된 데이터에 대한 액세스 권한을 잃지 않기 위해 2019년 5월 15일까지 키를 새 서비스 인스턴스로 마이그레이션해야 합니다. [{{site.data.keyword.cloud_notm}} 콘솔](https://{DomainName}/)에서 리소스 목록으로 이동하여 레거시 인스턴스를 사용하는지 여부를 확인할 수 있습니다. {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 {{site.data.keyword.cloud_notm}} 리소스 목록의 **Cloud Foundry Services** 섹션에 나열되어 있거나 서비스의 대상 오퍼레이션에 `https://ibm-key-protect.edge.bluemix.net` API 엔드포인트를 사용하는 경우, {{site.data.keyword.keymanagementserviceshort}}의 레거시 인스턴스를 사용하고 있는 것입니다. 2019년 5월 15일 후에는 레거시 엔드포인트에 더 이상 액세스할 수 없으며 오퍼레이션에 대해 서비스를 대상으로 지정할 수 없습니다.

암호화 키를 새 서비스 인스턴스로 마이그레이션하는 데 도움이 필요합니까? 자세한 단계는 [GitHub의 마이그레이션 클라이언트](https://github.com/IBM-Cloud/kms-migration-client){: external}를 참조하십시오. 마이그레이션 프로세스 또는 키의 상태에 대한 추가 질문이 있는 경우 Terry Mosbaugh([mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com))에게 문의하십시오.
{: tip}

## 2018년 12월
{: #dec-2018}

### 변경됨: {{site.data.keyword.keymanagementserviceshort}} API 엔드포인트
{: #changed-api-endpoints}

신규 기준일: 2018년 12월 19일

{{site.data.keyword.cloud_notm}}의 새 통합 경험에 맞추기 위해 {{site.data.keyword.keymanagementserviceshort}}가 해당 서비스 API의 기본 URL을 업데이트했습니다.

이제 새 `cloud.ibm.com` 엔드포인트를 참조하도록 애플리케이션을 업데이트할 수 있습니다.

- `keyprotect.us-south.bluemix.net`은 이제 `us-south.kms.cloud.ibm.com`입니다. 
- `keyprotect.us-east.bluemix.net`은 이제 `us-east.kms.cloud.ibm.com`입니다. 
- `keyprotect.eu-gb.bluemix.net`은 이제 `eu-gb.kms.cloud.ibm.com`입니다. 
- `keyprotect.eu-de.bluemix.net`은 이제 `eu-de.kms.cloud.ibm.com`입니다. 
- `keyprotect.au-syd.bluemix.net`은 이제 `au-syd.kms.cloud.ibm.com`입니다. 
- `keyprotect.jp-tok.bluemix.net`은 이제 `jp-tok.kms.cloud.ibm.com`입니다. 

현재 각 지역 서비스 엔드포인트에 대해 두 URL이 모두 지원됩니다. 

## 2018년 10월
{: #oct-2018}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 도쿄 지역으로 확장됨
{: #added-tokyo-region}

신규 기준일: 2018년 10월 31일

이제 도쿄 지역에 {{site.data.keyword.keymanagementserviceshort}} 리소스를 작성할 수 있습니다. 

자세한 정보는 [지역 및 위치](/docs/services/key-protect?topic=key-protect-regions)를 참조하십시오.

### 추가됨: {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인
{: #added-cli-plugin}

신규 기준일: 2018년 10월 2일

이제 {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인을 사용하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 키를 관리할 수 있습니다.

플러그인을 설치하는 방법에 대해 알아보려면 [CLI 설정](/docs/services/key-protect?topic=key-protect-set-up-cli)을 참조하십시오. {{site.data.keyword.keymanagementserviceshort}} CLI에 대해 자세히 알아보려면 [CLI 참조 문서를 확인](/docs/services/key-protect?topic=key-protect-cli-reference)하십시오.

## 2018년 9월
{: #sept-2018}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 요청 시 키 순환에 대한 지원을 추가함
{: #added-key-rotation}

신규 기준일: 2018년 9월 28일

이제 {{site.data.keyword.keymanagementserviceshort}}를 사용하여 요청 시 루트 키를 순환할 수 있습니다.

자세한 정보는 [키 순환](/docs/services/key-protect?topic=key-protect-rotate-keys)을 참조하십시오.

### 추가됨: 클라우드 앱에 필요한 엔드-투-엔드 보안 튜토리얼
{: #added-security-tutorial}

신규 기준일: 2018년 9월 14일

고유 암호화 키를 사용하여 스토리지 버킷 컨텐츠를 암호화하는 데 도움이 되는 코드 샘플을 찾고 계십니까?

이제 [새 튜토리얼](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)에 따라 클라우드 애플리케이션에 필요한 엔드-투-엔드 보안 추가를 연습할 수 있습니다.

자세한 정보는 [GitHub에서 샘플 앱을 확인](https://github.com/IBM-Cloud/secure-file-storage){: external}하십시오.

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 워싱턴 지역으로 확장됨
{: #added-wdc-region}

신규 기준일: 2018년 9월 10일

이제 워싱턴 지역에서 {{site.data.keyword.keymanagementserviceshort}} 리소스를 작성할 수 있습니다. 

자세한 정보는 [지역 및 위치](/docs/services/key-protect?topic=key-protect-regions)를 참조하십시오.

## 2018년 8월
{: #aug-2018}

### 변경됨: {{site.data.keyword.keymanagementserviceshort}} API 문서 URL
{: #changed-api-doc-url}

신규 기준일: 2018년 8월 28일

{{site.data.keyword.keymanagementserviceshort}} API 참조가 이동되었습니다! 

이제 다음 웹 사이트에서 API 문서에 액세스할 수 있습니다.
https://{DomainName}/apidocs/key-protect. 

## 2018년 3월
{: #mar-2018}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 프랑크푸르트 지역으로 확장됨
{: #added-frankfurt-region}

신규 기준일: 2018년 3월 21일

이제 프랑크푸르트 지역에 {{site.data.keyword.keymanagementserviceshort}} 리소스를 작성할 수 있습니다. 

자세한 정보는 [지역 및 위치](/docs/services/key-protect?topic=key-protect-regions)를 참조하십시오.

## 2018년 1월
{: #jan-2018}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 시드니 지역으로 확장됨
{: #added-sydney-region}

신규 기준일: 2018년 1월 31일

이제 시드니 지역에 {{site.data.keyword.keymanagementserviceshort}} 리소스를 작성할 수 있습니다. 

자세한 정보는 [지역 및 위치](/docs/services/key-protect?topic=key-protect-regions)를 참조하십시오.

## 2017년 12월
{: #dec-2017}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 BYOK(Bring Your Own Key)에 대한 지원을 추가함
{: #added-byok-support}

신규 기준일: 2017년 12월 15일

이제 {{site.data.keyword.keymanagementserviceshort}}가 BYOK(Bring Your Own Key) 및 고객 관리 암호화를 지원합니다.

- 고객 루트 키(CRK)라고도 하는 [루트 키](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types)를 서비스에 기본 리소스로 도입했습니다. 
- {{site.data.keyword.cos_full_notm}} 버킷에 대한 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how)를 사용으로 설정했습니다.

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 런던 지역으로 확장됨
{: #added-london-region}

신규 기준일: 2017년 12월 15일

이제 런던 지역에서 {{site.data.keyword.keymanagementserviceshort}}를 사용할 수 있습니다. 

자세한 정보는 [지역 및 위치](/docs/services/key-protect?topic=key-protect-regions)를 참조하십시오.

### 변경됨: {{site.data.keyword.iamshort}} 역할
{: #changed-iam-roles}

신규 기준일: 2017년 12월 15일

{{site.data.keyword.keymanagementserviceshort}} 리소스에 대해 수행할 수 있는 조치를 판별하는 {{site.data.keyword.iamshort}} 역할이 변경되었습니다.

- 이제 `Administrator`가 `Manager`임
- 이제 `Editor`가 `Writer`임
- 이제 `Viewer`가 `Reader`임

자세한 정보는 [사용자 액세스 관리](/docs/services/key-protect?topic=key-protect-manage-access)를 참조하십시오.

## 2017년 9월
{: #sept-2017}

### 추가됨: {{site.data.keyword.keymanagementserviceshort}}가 Cloud IAM에 대한 지원을 추가함
{: #added-iam-support}

신규 기준일: 2017년 9월 19일

이제 {{site.data.keyword.iamshort}}를 사용하여 {{site.data.keyword.keymanagementserviceshort}} 리소스에 대한 액세스 정책을 설정하고 관리할 수 있습니다.

자세한 정보는 [사용자 액세스 관리](/docs/services/key-protect?topic=key-protect-manage-access)를 참조하십시오.
