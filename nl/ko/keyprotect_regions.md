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

# 지역 및 위치
{: #regions-and-locations}

지역 서비스 엔드포인트를 지정하여 {{site.data.keyword.keymanagementservicelong_notm}} 서비스에 애플리케이션을 연결할 수 있습니다.
{: shortdesc}

## 사용 가능한 지역
{: #regions}

{{site.data.keyword.keymanagementserviceshort}}는 다음 지역에서 사용 가능합니다.

- 미국 남부
- 영국  

## 서비스 엔드포인트
{: #endpoints}

{{site.data.keyword.keymanagementserviceshort}} 리소스를 프로그래밍 방식으로 관리하는 경우 [{{site.data.keyword.keymanagementserviceshort}} API](https://console.ng.bluemix.net/apidocs/639)에 연결할 때 사용할 API 엔드포인트를 판별하려면 다음 표를 확인하십시오. 

<table>
    <tr>
        <th>지역</th>
        <th>API 엔드포인트</th>
    </tr>
    <tr>
        <td>미국 남부</td>
        <td>
            <code>https://keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>영국</td>
        <td>
            <code>https://keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">표 2. {{site.data.keyword.keymanagementserviceshort}} API에 사용 가능한 엔드포인트</caption>
</table>

**참고:** Cloud Foundry 조직 또는 영역 내에 있는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 대해 레거시 `https://ibm-key-protect.edge.bluemix.net` 엔드포인트를 사용하여 {{site.data.keyword.keymanagementserviceshort}} API와 상호작용하십시오.

{{site.data.keyword.keymanagementserviceshort}} 인증에 대한 정보는 [API에 액세스](/docs/services/keymgmt/keyprotect_authentication.html)를 참조하십시오.
