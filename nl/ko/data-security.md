---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: data security, Key Protect compliance, encryption key deletion

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

# 보안 및 규제 준수
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}}에는 규제 준수 요구를 충족시키고 클라우드에서 데이터를 안전하게 보호하기 위한 데이터 보안 전략이 있습니다.
{: shortdesc}

## 데이터 보안 및 암호화
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}}는 [{{site.data.keyword.cloud_notm}} HSM(Hardware Security Module) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/cloud/hardware-security-module)을 사용하여 제공자 관리 키 자료를 생성하고 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-envelope-encryption) 오퍼레이션을 수행합니다. HSM은 암호화 경계 밖에 키를 노출하지 않고 암호화 키 자료를 저장하고 사용하는 위조 방지 하드웨어 디바이스입니다.

서비스에 대한 액세스는 HTTPS를 통해 이루어지며, 내부 {{site.data.keyword.keymanagementserviceshort}} 통신은 TLS(Transport Layer Security) 1.2 프로토콜을 사용하여 이동 중인 데이터를 암호화합니다.

{{site.data.keyword.keymanagementserviceshort}}가 정부 규제 준수를 충족시키는 방법에 대해 자세히 알아보려면 [플랫폼 및 서비스 규제 준수](/docs/overview?topic=overview-security#compliancetable)를 참조하십시오.

## 데이터 삭제
{: #data-deletion}

키를 삭제하면 서비스가 키를 삭제됨으로 표시하고 키는 _영구 삭제됨_ 상태로 전이됩니다. 이 상태의 키는 더 이상 복구 불가능하며 키를 사용하는 클라우드 서비스는 키와 연관된 데이터를 더 이상 복호화할 수 없습니다. 데이터는 암호화된 양식으로 해당 서비스에 남아 있습니다. 키와 연관된 메타데이터(예: 키의 상태 전이 히스토리 및 이름)가 {{site.data.keyword.keymanagementserviceshort}} 데이터베이스에 보관됩니다. 

{{site.data.keyword.keymanagementserviceshort}}에서 키를 삭제하는 것은 파괴적인 오퍼레이션입니다. 키를 삭제한 후에는 조치를 되돌릴 수 없으며 키와 연관된 모든 데이터는 키가 삭제되는 즉시 유실됩니다. 키를 삭제하기 전에 키와 연관된 데이터를 검토하고 이 데이터에 더 이상 액세스할 필요가 없는지 확인하십시오. 프로덕션 환경에서 활발하게 데이터를 보호하고 있는 키는 삭제하지 마십시오. 

키로 보호되는 데이터를 판별하는 데 도움이 되도록 `ibmcloud iam authorization-policies`를 실행하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 기존 클라우드 서비스에 맵핑하는 방법을 볼 수 있습니다. 콘솔에서 서비스 권한을 보는 방법을 자세히 보려면 [서비스 간 액세스 권한 부여](/docs/iam?topic=iam-serviceauth)를 참조하십시오.
{: note}
