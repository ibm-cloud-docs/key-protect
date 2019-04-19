---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Key Protect availability, Key Protect disaster recovery

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

# 고가용성 및 재해 복구
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}}는 애플리케이션이 계속 보안 유지되고 가동하는 데 도움이 되는 자동 기능을 포함한 고가용성의 지역 서비스입니다.
{: shortdesc}

이 페이지에서 {{site.data.keyword.keymanagementserviceshort}}의 가용성 및 재해 복구 전략에 대해 자세히 알아보십시오.

## 위치, 테넌시 및 가용성
{: #availability}

{{site.data.keyword.keymanagementserviceshort}}는 멀티테넌트 지역 서비스입니다. 

{{site.data.keyword.keymanagementserviceshort}} 요청이 처리되는 지리적 지역을 나타내는, 지원되는 [{{site.data.keyword.cloud_notm}} 지역](/docs/services/key-protect?topic=key-protect-regions#regions) 중 하나에서 {{site.data.keyword.keymanagementserviceshort}} 리소스를 작성할 수 있습니다. 각 {{site.data.keyword.cloud_notm}} 지역에는 로컬 액세스, 짧은 대기 시간 및 지역에 대한 보안 요구사항을 충족시키기 위한 [다중 가용성 구역 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/)이 포함됩니다.

{{site.data.keyword.cloud_notm}}에서 저장 암호화 전략을 계획할 때 가장 가까운 지역에서 {{site.data.keyword.keymanagementserviceshort}}를 프로비저닝하면 {{site.data.keyword.keymanagementserviceshort}} API와 상호작용할 때 연결이 빨라지고 신뢰성이 높아질 수 있다는 점을 유념하십시오. {{site.data.keyword.keymanagementserviceshort}} 리소스에 의존하는 사용자, 앱 또는 서비스가 지리적으로 집중되어 있는 경우 특정 지역을 선택하십시오. 지역에서 멀리 떨어진 사용자 및 서비스의 대기 시간이 길어질 수 있다는 점을 기억하십시오. 

암호화 키는 이 암호화 키가 작성된 지역으로 한정됩니다. {{site.data.keyword.keymanagementserviceshort}}는 암호화 키를 복사하거나 다른 지역으로 내보내지 않습니다.
{: note}

## 재해 복구
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}}에는 RTO(Recovery Time Objective)가 한 시간인 지역 재해 복구가 있습니다. 서비스는 재해 이벤트에서 계획하고 복구하기 위한 {{site.data.keyword.cloud_notm}} 요구사항을 따릅니다. 자세한 정보는 [재해 복구](/docs/overview?topic=overview-zero-downtime#disaster-recovery)를 참조하십시오.


