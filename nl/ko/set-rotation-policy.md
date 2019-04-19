---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# 순환 정책 설정
{: #set-rotation-policy}

{{site.data.keyword.keymanagementservicefull}}를 사용하여 루트 키에 대한 자동 순환 정책을 설정할 수 있습니다. 
{: shortdesc}

루트 키에 대한 자동 순환 정책을 설정하는 경우 정기적으로 키의 수명을 줄이고 이 키로 보호되는 정보의 양을 제한합니다.

{{site.data.keyword.keymanagementserviceshort}}에서 생성된 루트 키에 대해서만 순환 정책을 작성할 수 있습니다. 초기에 루트 키를 가져온 경우 base64로 인코딩된 새 키 자료를 제공하여 키를 순환해야 합니다. 자세한 정보는 [요청 시 루트 키 순환](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys)을 참조하십시오.
{: note}

{{site.data.keyword.keymanagementserviceshort}}에서 키 순환 옵션에 대해 자세히 보시겠습니까? 자세한 정보는 [키 순환 옵션 비교](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)를 참조하십시오.
{: tip}

## GUI에서 순환 정책 관리
{: #manage-policies-gui}

그래픽 인터페이스를 사용하여 루트 키에 대한 정책을 관리하려는 경우 {{site.data.keyword.keymanagementserviceshort}} GUI를 사용할 수 있습니다.

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에 로그인](https://{DomainName}/){: new_window}하십시오.
2. **메뉴** &gt; **리소스 목록**으로 이동하여 리소스 목록을 보십시오.
3. {{site.data.keyword.cloud_notm}} 리소스 목록에서 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스를 선택하십시오.
4. 애플리케이션 세부사항 페이지에서 **키** 테이블을 사용하여 서비스에서 키를 찾아보십시오.
5. ⋯ 아이콘을 클릭하여 특정 키에 대한 옵션 목록을 여십시오.
6. 옵션 메뉴에서 **정책 관리**를 클릭하여 키에 대한 순환 정책을 관리하십시오.
7. 순환 옵션 목록에서 월 단위로 순환 빈도를 선택하십시오.

    키에 기존 순환 정책이 있는 경우 인터페이스는 키의 기존 순환 기간을 표시합니다.

8. **정책 작성**을 클릭하여 키에 대한 정책을 설정하십시오.

지정한 순환 간격에 따라 키를 순환할 때가 되면 {{site.data.keyword.keymanagementserviceshort}}가 루트 키를 새 키 자료로 자동으로 바꿉니다.

## API로 순환 정책 관리
{: #manage-rotation-policies-api}

### 순환 정책 보기
{: #view-rotation-policy-api}

상위 레벨 보기의 경우 다음 엔드포인트에 대한 `GET` 호출을 작성하여 루트 키와 연관된 정책을 찾아볼 수 있습니다.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [서비스 및 인증용 인증 정보를 검색](/docs/services/key-protect?topic=key-protect-set-up-api)하십시오.

2. 다음 cURL 명령을 실행하여 지정된 키에 대한 순환 정책을 검색하십시오.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.
    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>필수.</strong> 기존 순환 정책이 있는 루트 키의 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 상주하는 지리적 영역을 표시하는 지역 약어(예: <code>us-south</code> 또는 <code>eu-gb</code>)입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">지역 서비스 엔드포인트</a>를 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>필수.</strong> 사용자의 {{site.data.keyword.cloud_notm}} 액세스 토큰입니다. cURL 요청에 Bearer 값 등 <code>IAM</code> 토큰의 전체 컨텐츠를 포함하십시오. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">액세스 토큰 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">인스턴스 ID 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
        <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 순환 정책을 작성하는 데 필요한 변수에 대한 설명</caption>
    </table>

    성공한 `GET api/v2/keys/{id}/policies` 응답은 키와 연관된 정책 세부사항을 리턴합니다. 다음 JSON 오브젝트는 기존 순환 정책이 있는 루트 키에 대한 응답 예를 표시합니다.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    `interval_month` 값은 월 단위 키 순환 빈도를 표시합니다.

### 순환 정책 작성
{: #create-rotation-policy-api}

다음 엔드포인트에 대한 `PUT` 호출을 작성하여 루트 키에 대한 순환 정책을 작성하십시오.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [서비스 및 인증용 인증 정보를 검색](/docs/services/key-protect?topic=key-protect-set-up-api)하십시오.

2. 다음 cURL 명령을 실행하여 지정된 키에 대한 순환 정책을 작성하십시오.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.
    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>필수.</strong> 순환 정책을 작성할 루트 키의 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 상주하는 지리적 영역을 표시하는 지역 약어(예: <code>us-south</code> 또는 <code>eu-gb</code>)입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">지역 서비스 엔드포인트</a>를 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>필수.</strong> 사용자의 {{site.data.keyword.cloud_notm}} 액세스 토큰입니다. cURL 요청에 Bearer 값 등 <code>IAM</code> 토큰의 전체 컨텐츠를 포함하십시오. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">액세스 토큰 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">인스턴스 ID 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>rotation_interval</varname></td>
        <td><strong>필수.</strong> 월 단위 키 순환 간격을 판별하는 정수 값입니다. 최소값은 <code>1</code>이며 최대값은 <code>12</code>입니다.
        </td>
      </tr>
        <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 순환 정책을 작성하는 데 필요한 변수에 대한 설명</caption>
    </table>

    성공한 `PUT api/v2/keys/{id}/policies` 응답은 키와 연관된 정책 세부사항을 리턴합니다. 다음 JSON 오브젝트는 기존 순환 정책이 있는 루트 키에 대한 응답 예를 표시합니다.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### 순환 정책 업데이트
{: #update-rotation-policy-api}

다음 엔드포인트에 대한 `PUT` 호출을 작성하여 루트 키에 대한 기존 정책을 업데이트하십시오.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [서비스 및 인증용 인증 정보를 검색](/docs/services/key-protect?topic=key-protect-set-up-api)하십시오.

2. 다음 cURL 명령을 실행하여 지정된 키에 대한 순환 정책을 바꾸하십시오.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.
    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>필수.</strong> 순환 정책을 바꿀 루트 키의 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 상주하는 지리적 영역을 표시하는 지역 약어(예: <code>us-south</code> 또는 <code>eu-gb</code>)입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">지역 서비스 엔드포인트</a>를 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>필수.</strong> 사용자의 {{site.data.keyword.cloud_notm}} 액세스 토큰입니다. cURL 요청에 Bearer 값 등 <code>IAM</code> 토큰의 전체 컨텐츠를 포함하십시오. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">액세스 토큰 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. 자세한 정보는 <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">인스턴스 ID 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>필수.</strong> 월 단위 키 순환 간격을 판별하는 정수 값입니다. 최소값은 <code>1</code>이며 최대값은 <code>12</code>입니다.
        </td>
      </tr>
        <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 순환 정책을 작성하는 데 필요한 변수에 대한 설명</caption>
    </table>

    성공한 `PUT api/v2/keys/{id}/policies` 응답은 키와 연관된 업데이트된 정책 세부사항을 리턴합니다. 다음 JSON 오브젝트는 업데이트된 순환 정책이 있는 루트 키에 대한 응답 예를 표시합니다.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    `interval_month` 및 `updatedat` 값이 키에 대한 정책 세부사항에서 업데이트됩니다. 다른 사용자가 처음에 작성한 키에 대한 정책을 업데이트하는 경우에도 `updatedby` 값이 요청을 보낸 사용자의 ID를 표시하도록 변경됩니다.
