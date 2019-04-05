---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

다음 FAQ를 사용하여 {{site.data.keyword.keymanagementservicelong}}에 대한 도움을 받을 수 있습니다.

## {{site.data.keyword.keymanagementserviceshort}}의 가격은 어떻게 책정됩니까?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}}는 20개 이하의 키가 필요한 사용자를 위해 무료 요금으로 [등급별 티어 플랜](https://{DomainName}/catalog/services/key-protect)을 제공합니다. {{site.data.keyword.cloud_notm}} 계정마다 20개의 무료 키를 가질 수 있습니다. 팀에 {{site.data.keyword.keymanagementserviceshort}}의 다중 인스턴스가 필요한 경우, {{site.data.keyword.cloud_notm}}는 계정 내 모든 인스턴스에 활성 키를 추가한 다음 가격을 적용합니다. 

## 활성 암호화 키는 무엇입니까?
{: #what-is-active-encryption-key}
{: faq}

{{site.data.keyword.keymanagementserviceshort}}로 암호화 키를 가져오거나 {{site.data.keyword.keymanagementserviceshort}}를 사용하여 HSM에서 키를 생성하는 경우 해당 키는 _활성_ 키가 됩니다. 가격은 {{site.data.keyword.cloud_notm}} 계정 내 모든 활성 키를 기준으로 합니다. 

## 내 키를 그룹화하고 관리하는 방법은 무엇입니까?
{: #how-to-group-keys}
{: faq}

가격 면에서 볼 때, {{site.data.keyword.keymanagementserviceshort}}를 사용하는 최선의 방법은 제한된 수의 루트 키를 작성한 다음 이 루트 키를 사용하여 외부 앱 또는 클라우드 데이터 서비스에서 작성한 데이터 암호화 키를 암호화하는 것입니다. 

루트 키를 사용하여 데이터 암호화 키를 보호하는 방법에 대해 자세히 알아보려면 [엔벨로프 암호화로 데이터 보호](/docs/services/key-protect?topic=key-protect-envelope-encryption)를 참조하십시오.

## 루트 키는 무엇입니까?
{: #what-is-root-key}
{: faq}

루트 키는 {{site.data.keyword.keymanagementserviceshort}}의 기본 리소스입니다. 이는 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-envelope-encryption)를 사용하여 데이터 서비스에 저장된 다른 키를 보호하기 위해 신뢰 루트로 사용되는 대칭 키-랩핑 키입니다. {{site.data.keyword.keymanagementserviceshort}}를 사용하면 루트 키의 라이프사이클을 작성하고 저장하고 관리하여 클라우드에 저장된 다른 키를 완전히 제어할 수 있습니다. 

## 엔벨로프 암호화는 무엇입니까?
{: #what-is-envelope-encryption}
{: faq}

엔벨로프 암호화는 _데이터 암호화 키_로 데이터를 암호화한 다음 보안성이 높은 _키-랩핑 키_로 데이터 암호화 키를 암호화하는 것입니다. 저장 데이터는 여러 계층의 암호화를 적용하여 보호됩니다. {{site.data.keyword.cloud_notm}} 리소스에 엔벨로프 암호화를 사용하는 방법에 대해 알아보려면 [서비스 통합](/docs/services/key-protect?topic=key-protect-integrate-services)을 참조하십시오.

## 키 이름의 길이는 얼마입니까?
{: #key-names}
{: faq}

최대 90자 길이의 키 이름을 사용할 수 있습니다.

## 개인 정보를 내 키의 메타데이터로 저장할 수 있습니까?
{: #personal-data}
{: faq}

개인 데이터의 기밀성을 보호하려면 PII(Personally Identifiable Information)를 키의 메타데이터로 저장하지 마십시오. 개인 정보에는 사용자의 이름, 주소, 전화번호, 이메일 주소 또는 사용자, 사용자의 고객 또는 다른 사용자를 식별하거나 연락하거나 찾을 수 있는 기타 정보가 포함됩니다.

{{site.data.keyword.keymanagementserviceshort}} 리소스 및 암호화 키의 메타데이터로 저장하는 정보에 대한 보안을 보장해야 합니다. 개인 데이터에 대한 추가 예제는 [NIST Special Publication 800-122 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}의 섹션 2.2를 참조하십시오.
{: important}

## 한 지역에서 작성된 키를 다른 지역에서 사용할 수 있습니까?
{: #keys-across-regions}
{: faq}

암호화 키는 이 암호화 키가 작성된 지역으로 한정됩니다. {{site.data.keyword.keymanagementserviceshort}}는 암호화 키를 복사하거나 다른 지역으로 내보내지 않습니다.

## 키에 대한 액세스 권한이 있는 사용자를 제어하는 방법은 무엇입니까?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}}는 암호화 키에 대한 액세스 및 사용자 관리에 도움이 되도록 {{site.data.keyword.iamlong}}에서 관리하는 중앙 집중식 액세스 제어 시스템을 지원합니다. 서비스에 대한 보안 관리자인 경우 팀의 구성원에게 부여할 [특정 {{site.data.keyword.keymanagementserviceshort}} 권한에 대응되는 Cloud IAM 역할](/docs/services/key-protect?topic=key-protect-manage-access#roles)을 지정할 수 있습니다.

## {{site.data.keyword.keymanagementserviceshort}}에 대한 API 호출을 모니터하는 방법은 무엇입니까?
{: faq}

{{site.data.keyword.cloudaccesstrailfull_notm}} 서비스를 사용하여 사용자 및 애플리케이션이 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스와 상호작용하는 방법을 추적할 수 있습니다. 예를 들어, {{site.data.keyword.keymanagementserviceshort}}에서 키 작성, 가져오기, 삭제 또는 읽기를 수행할 때 {{site.data.keyword.cloudaccesstrailshort}} 이벤트가 생성됩니다. 이러한 이벤트는 {{site.data.keyword.keymanagementserviceshort}} 서비스가 프로비저닝된 지역과 동일한 지역의 {{site.data.keyword.cloudaccesstrailshort}} 서비스에 자동으로 전달됩니다.

자세한 내용을 알아보려면 [활동 트래커 이벤트](/docs/services/key-protect?topic=key-protect-activity-tracker-events)를 참조하십시오.

## 키를 삭제하면 어떻게 됩니까?
{: #key-destruction}
{: faq}

키를 삭제하면 서비스가 키를 삭제됨으로 표시하고 키는 _영구 삭제됨_ 상태로 전이됩니다. 이 상태의 키는 더 이상 복구 불가능하며 키를 사용하는 클라우드 서비스는 키와 연관된 데이터를 더 이상 복호화할 수 없습니다. 데이터는 암호화된 양식으로 해당 서비스에 남아 있습니다. 키와 연관된 메타데이터(예: 키의 상태 전이 히스토리 및 이름)가 {{site.data.keyword.keymanagementserviceshort}} 데이터베이스에 보관됩니다. 

키를 삭제하기 전에 키와 연관된 데이터에 더 이상 액세스할 필요가 없는지 확인하십시오. 이 조치는 되돌릴 수 없습니다.

## 내 서비스 인스턴스를 디프로비저닝해야 할 때 어떤 일이 발생합니까?
{: #deprovision-service}
{: faq}

{{site.data.keyword.keymanagementserviceshort}}를 사용하지 않기로 결정하는 경우, 서비스를 디프로비저닝하려면 먼저 서비스 인스턴스에서 남은 키를 삭제해야 합니다. 서비스 인스턴스를 삭제한 후 {{site.data.keyword.keymanagementserviceshort}}는 [엔벨로프 암호화](/docs/services/key-protect?topic=key-protect-envelope-encryption)를 사용하여 서비스 인스턴스와 연관된 모든 데이터의 암호를 파쇄합니다. 
