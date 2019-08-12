---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# 보안 및 규제 준수
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}}에는 규제 준수 요구를 충족시키고 클라우드에서 데이터를 안전하게 보호하기 위한 데이터 보안 전략이 있습니다.
{: shortdesc}

## 보안 준비
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}}는 시스템, 네트워킹 및 보안 엔지니어링에 대한 {{site.data.keyword.IBM_notm}} 우수 사례를 준수함으로써 보안 준비를 보장합니다.  

{{site.data.keyword.cloud_notm}}의 보안 제어에 대해 자세히 알아보려면 [데이터가 안전한지 확인하는 방법](/docs/overview?topic=overview-security#security)을 참조하십시오.
{: tip}

### 데이터 암호화
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}}는 [{{site.data.keyword.cloud_notm}} HSM(Hardware Security Module)](https://www.ibm.com/cloud/hardware-security-module){: external}을 사용하여 제공자 관리 키 자료를 생성하고 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-envelope-encryption) 오퍼레이션을 수행합니다. HSM은 암호화 경계 밖에 키를 노출하지 않고 암호화 키 자료를 저장하고 사용하는 위조 방지 하드웨어 디바이스입니다.

서비스에 대한 액세스는 HTTPS를 통해 이루어지며, 내부 {{site.data.keyword.keymanagementserviceshort}} 통신은 TLS(Transport Layer Security) 1.2 프로토콜을 사용하여 이동 중인 데이터를 암호화합니다.

### 데이터 삭제
{: #data-deletion}

키를 삭제하면 서비스가 키를 삭제됨으로 표시하고 키는 _영구 삭제됨_ 상태로 전이됩니다. 이 상태의 키는 더 이상 복구 불가능하며 키를 사용하는 클라우드 서비스는 키와 연관된 데이터를 더 이상 복호화할 수 없습니다. 데이터는 암호화된 양식으로 해당 서비스에 남아 있습니다. 키와 연관된 메타데이터(예: 키의 상태 전이 히스토리 및 이름)가 {{site.data.keyword.keymanagementserviceshort}} 데이터베이스에 보관됩니다. 

{{site.data.keyword.keymanagementserviceshort}}에서 키를 삭제하는 것은 파괴적인 오퍼레이션입니다. 키를 삭제한 후에는 조치를 되돌릴 수 없으며 키와 연관된 모든 데이터는 키가 삭제되는 즉시 유실됩니다. 키를 삭제하기 전에 키와 연관된 데이터를 검토하고 이 데이터에 더 이상 액세스할 필요가 없는지 확인하십시오. 프로덕션 환경에서 활발하게 데이터를 보호하고 있는 키는 삭제하지 마십시오. 

키로 보호되는 데이터를 판별하는 데 도움이 되도록 `ibmcloud iam authorization-policies`를 실행하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 기존 클라우드 서비스에 맵핑하는 방법을 볼 수 있습니다. 콘솔에서 서비스 권한을 보는 방법을 자세히 보려면 [서비스 간 액세스 권한 부여](/docs/iam?topic=iam-serviceauth)를 참조하십시오.
{: note}

## 규제 준수 준비
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}}는 GDPR, HIPAA 및 ISO 27001/27017/27018 등을 포함하여 글로벌, 산업 및 지역 규제 준수 표준에 대한 제어를 충족합니다.  

전체 {{site.data.keyword.cloud_notm}} 규제 준수 인증 목록은 [{{site.data.keyword.cloud_notm}}의 규제 준수](https://www.ibm.com/cloud/compliance){: external}를 참조하십시오.
{: tip}

### EU 지원
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}}는 유럽 연합(EU)에서 {{site.data.keyword.keymanagementserviceshort}} 리소스를 보호하기 위한 추가 제어 기능을 갖추고 있습니다.  

유럽에 있는 사용자의 개인 데이터를 처리하기 위해 독일 프랑크푸르트에서 {{site.data.keyword.keymanagementserviceshort}} 리소스를 사용할 경우, {{site.data.keyword.cloud_notm}} 계정에 대한 EU 지원 설정을 사용할 수 있습니다. 더 자세히 알아보려면 [EU 지원 설정 사용](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) 및 [EU의 리소스에 대한 지원 요청](/docs/get-support?topic=get-support-getting-customer-support#eusupported)의 내용을 참조하십시오.

### 일반 개인정보 보호법률(General Data Protection Regulation, GDPR)
{: #gdpr}

GDPR의 목적은 EU 전체에 적용되는 일관된 개인정보 보호 법 체계를 만들어, 시민들에게 자신의 개인 정보에 대한 통제권을 돌려주는 동시에 전 세계에서 이러한 정보를 호스팅하고 처리하는 대상에게 엄격한 규칙을 적용하는 것입니다.


{{site.data.keyword.IBM_notm}}은 고객과 {{site.data.keyword.IBM_notm}} 비즈니스 파트너가 GDPR에 대비하는데 도움을 줄 수 있도록 혁신적인 데이터 개인정보 보호, 보안 및 통제 솔루션을 제공하려 노력하고 있습니다.

{{site.data.keyword.keymanagementserviceshort}} 리소스에 대한 GDPR 규제 준수를 보장하려면 {{site.data.keyword.cloud_notm}} 계정에 대한 [EU 지원 설정을 사용](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)으로 설정하십시오. {{site.data.keyword.keymanagementserviceshort}}에서 다음 부록을 검토하여 개인 데이터를 처리 및 보호하는 방법에 대해 알아볼 수 있습니다. 

- [{{site.data.keyword.keymanagementservicefull_notm}} DSA(Data Sheet Addendum)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} DPA(Data Processing Addendum)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA 지원
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}}는 PHI(Protected Health Information)를 안전하게 보호하기 위해 미국 건강보험 이전과 책임에 관한 법(HIPAA)에 대한 제어를 충족합니다. 

귀하 또는 귀사가 HIPAA에서 정의하는 대상 법인인 경우에는 {{site.data.keyword.cloud_notm}} 계정에 대한 HIPPA 지원 설정을 사용으로 설정할 수 있습니다. 더 자세히 알아보려면 [HIPAA 지원 설정 사용](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa)을 참조하십시오.

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} - ISO 27001, 27017, 27018 인증입니다. [{{site.data.keyword.cloud_notm}}의 규제 준수](https://www.ibm.com/cloud/compliance){: external}를 방문하면 규제 준수 인증을 볼 수 있습니다. 

### SOC 2 Type 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} - SOC 2 Type 1 인증입니다. {{site.data.keyword.cloud_notm}} SOC 2 보고서 요청에 대한 정보는 [{{site.data.keyword.cloud_notm}}의 규제 준수](https://www.ibm.com/cloud/compliance){: external}를 참조하십시오.
