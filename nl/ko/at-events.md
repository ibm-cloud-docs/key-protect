---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# 활동 트래커 이벤트
{: #at-events}

보안 담당자, 감사자 또는 관리자는 활동 트래커 서비스를 사용하여 사용자 및 애플리케이션이 {{site.data.keyword.keymanagementservicefull}}와 상호작용하는 방법을 추적할 수 있습니다.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

활동 트래커는 {{site.data.keyword.cloud_notm}}에 있는 서비스의 상태를 변경하는 사용자 시작 활동을 기록합니다. 이 서비스를 사용하여 비정상 활동 및 치명적인 조치를 조사하고 규제 감사 요구사항을 준수할 수 있습니다. 또한 발생 시에 조치에 대한 경보가 표시될 수 있습니다. 수집되는 이벤트는 CADF(Cloud Auditing Data Federation) 표준을 준수합니다. 

{{site.data.keyword.cloud_notm}} 카탈로그에서 사용할 수 있는 활동 트래커 서비스는 현재 두 개입니다. {{site.data.keyword.keymanagementserviceshort}}는 이벤트를 두 서비스 모두에 보내며, 사용자는 서비스를 사용하여 {{site.data.keyword.cloud_notm}}에서 {{site.data.keyword.keymanagementserviceshort}} 활동을 모니터할 수 있습니다. 그러나 {{site.data.keyword.cloudaccesstrailfull}}의 경우 더 이상 사용되지 않고 새 인스턴스를 작성할 수 없으며, 기존 서비스 인스턴스에 대한 지원은 2019년 9월 30일까지만 사용 가능합니다.

자세한 정보는 다음을 참조하십시오. 
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)(더 이상 사용되지 않음)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## 이벤트 목록
{: #at-actions}

다음 표에는 이벤트를 생성하는 {{site.data.keyword.keymanagementserviceshort}} 조치가 나열되어 있습니다. 

|조치                   |설명                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     |키 작성                |
| `kms.secrets.read`       |ID별로 키 검색        |
| `kms.secrets.delete`     |ID별로 키 삭제          |
| `kms.secrets.list`       |키 목록 검색     |
| `kms.secrets.head`       |키의 수 검색 |
| `kms.secrets.wrap`       |키 랩핑                  |
| `kms.secrets.unwrap`     |키 랩핑 해제                |
| `kms.policies.read`      |    키에 대한 정책 보기     |
| `kms.policies.write`     |    키에 대한 정책 설정      |
{: caption="표 1. 활동 트래커 이벤트를 생성하는 {{site.data.keyword.keymanagementserviceshort}} 조치" caption-side="top"}

## 이벤트 보기
{: #at-ui}

[{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) 또는 [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started)(더 이상 사용되지 않음)를 사용하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스와 연관된 활동 트래커 이벤트를 볼 수 있습니다.

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### {{site.data.keyword.at_full_notm}} 사용
{: #at-ui-logdna}

{{site.data.keyword.keymanagementserviceshort}}의 인스턴스에 의해 생성된 이벤트는 자동으로 동일한 위치에서 사용할 수 있는 {{site.data.keyword.at_full_notm}} 서비스 인스턴스로 전달됩니다.  

{{site.data.keyword.at_full_notm}}의 경우 위치당 하나의 인스턴스만 가질 수 있습니다. 이벤트를 보려면 서비스 인스턴스를 사용할 수 있는 동일한 위치에서 {{site.data.keyword.at_full_notm}} 서비스의 웹 UI에 액세스해야 합니다. 자세한 정보는 [IBM Cloud UI를 통해 웹 UI 실행](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2)을 참조하십시오.

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### {{site.data.keyword.cloudaccesstrailfull_notm}} 사용(더 이상 사용되지 않음)
{: #at-ui-legacy}

{{site.data.keyword.cloudaccesstrailshort}} 이벤트는 이벤트가 생성된 {{site.data.keyword.cloud_notm}} 지역에서 사용 가능한 {{site.data.keyword.cloudaccesstrailshort}} **계정 도메인**에서 사용할 수 있습니다. 자세한 정보는 [이벤트 보기](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4)를 참조하십시오.


## 이벤트 분석
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

암호화 키에 대한 정보의 민감도로 인해 {{site.data.keyword.keymanagementserviceshort}}에 대한 API 호출의 결과로 이벤트가 생성될 때 생성된 이벤트에는 키에 대한 자세한 정보가 포함되지 않습니다. 이 이벤트에는 클라우드 환경에서 내부적으로 키를 식별하는 데 사용할 수 있는 상관 ID가 포함됩니다. 상관 ID는 `responseBody.content` 필드의 일부로 리턴되는 필드입니다. 이 정보를 사용하여 {{site.data.keyword.cloudaccesstrailshort}} 이벤트를 통해 보고되는 조치의 정보와 {{site.data.keyword.keymanagementserviceshort}} 키를 상관시킬 수 있습니다.
