---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 지역 및 위치
{: #regions}

지역 서비스 엔드포인트를 지정하여 {{site.data.keyword.keymanagementservicelong_notm}} 서비스에 애플리케이션을 연결할 수 있습니다.
{: shortdesc}

## 사용 가능한 지역
{: #regions}

{{site.data.keyword.keymanagementserviceshort}}는 다음과 같은 지역과 위치에서 사용 가능합니다. ![이 이미지는 Key Protect 서비스가 사용 가능한 지역을 보여줍니다.](images/world-map_min.svg)

## 서비스 엔드포인트
{: #endpoints}

{{site.data.keyword.keymanagementserviceshort}} 리소스를 프로그래밍 방식으로 관리하는 경우 [{{site.data.keyword.keymanagementserviceshort}} API](https://console.bluemix.net/apidocs/kms)에 연결할 때 사용할 API 엔드포인트를 판별하려면 다음 표를 확인하십시오. 

<table>
    <tr>
        <th>지역 이름</th>
        <th>지리적 위치</th>
        <th>서비스 API 엔드포인트</th>
    </tr>
    <tr>
        <td>독일</td>
        <td>프랑크푸르트, 독일</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Sydney</td>
        <td>시드니, 호주</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>영국</td>
        <td>런던, 영국</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>미국 남부</td>
        <td>댈러스, 미국</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API에 사용 가능한 엔드포인트를 보여줍니다.</caption>
</table>

Cloud Foundry 조직 또는 영역 내에 있는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스의 경우 레거시 `https://ibm-key-protect.edge.bluemix.net` 엔드포인트를 사용하여 {{site.data.keyword.keymanagementserviceshort}} API와 상호작용하십시오.
{: tip}

{{site.data.keyword.keymanagementserviceshort}} 인증에 대한 자세한 정보는 [API에 액세스](/docs/services/key-protect/access-api.html)를 참조하십시오.
