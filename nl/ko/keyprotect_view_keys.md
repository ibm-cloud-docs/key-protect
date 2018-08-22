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

# 키 보기
{: #viewing-keys}

{{site.data.keyword.keymanagementservicefull}}는 암호화 키를 확인, 관리, 감사할 수 있는 중앙 집중식 시스템을 제공합니다. 키와 키에 대한 액세스 제한사항을 감사하여 자원의 보안을 유지하십시오.
{: shortdesc}

정기적으로 키 구성 감사:

- 키가 작성된 시점을 확인하고 키를 순환할 시점인지 판별합니다.
- [{{site.data.keyword.cloudaccesstrailshort}}로 {{site.data.keyword.keymanagementserviceshort}}에 대한 API 호출을 모니터합니다. ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/services/cloud-activity-tracker/svcs/kp_at.html#kp_at){: new_window}
- 어떤 사용자에게 키에 대한 액세스 권한이 있고, 해당 액세스 레벨은 적합한지 검사합니다.

리소스에 대한 액세스 감사에 대한 자세한 정보는 [Cloud IAM으로 사용자 액세스 관리](/docs/services/keymgmt/keyprotect_manage_access.html)를 참조하십시오.

## GUI로 키 보기
{: #gui}

그래픽 인터페이스를 사용한 서비스의 키 검사를 원하는 경우 {{site.data.keyword.keymanagementserviceshort}} 대시보드를 사용할 수 있습니다.

[키를 작성하거나 기존 키를 서비스로 가져온 후](/docs/services/keymgmt/keyprotect_create_keys.html) 다음 단계를 완료하여 키를 확인하십시오.

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/)에 로그인하십시오.
2. {{site.data.keyword.cloud_notm}} 대시보드에서 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스를 선택하십시오.
3. {{site.data.keyword.keymanagementserviceshort}} 대시보드에서 키의 일반 특성을 찾아보십시오.

    <table>
      <tr>
        <th>열</th>
        <th>설명</th>
      </tr>
      <tr>
        <td>이름</td>
        <td>키에 지정한, 사용자가 읽을 수 있는 고유한 별명입니다.</td>
      </tr>
      <tr>
        <td>ID</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} 서비스에서 키에 지정한 고유 키 ID입니다. [{{site.data.keyword.keymanagementserviceshort}} API](https://console.ng.bluemix.net/apidocs/639)를 사용하여 서비스에 대한 호출을 작성하는 데 ID 값을 사용할 수 있습니다.</td>
      </tr>
      <tr>
        <td>시/도</td>
        <td>[NIST Special Publication 800-57, Recommendation for Key Management![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf)를 기반으로 한 [키 상태](/docs/services/keymgmt/keyprotect_states.html)입니다. 이러한 상태는 <i>활성화 이전</i>, <i>활성</i>, <i>비활성화됨</i> 및 <i>영구 삭제됨</i>입니다.</td>
      </tr>
      <tr>
        <td>유형</td>
        <td>서비스 내 키의 지정된 용도에 대해 설명하는 [키 유형](/docs/services/keymgmt/keyprotect_envelope.html#key_types)입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. <b>키</b> 테이블에 대한 설명</caption>
    </table>

## API로 키 보기
{: #api}

{{site.data.keyword.keymanagementserviceshort}} API를 사용하여 키의 컨텐츠를 검색할 수 있습니다.

### 1단계: 키의 콜렉션 검색
{: #retrieve_keys_api}

상위 레벨 보기의 경우에는 다음 엔드포인트에 대한 `GET` 호출을 작성하여 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스에서 관리되는 키를 찾아볼 수 있습니다.

```
https://key-protect.us-south.bluemix.net/api/v2/keys
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/keymgmt/keyprotect_authentication.html)
2. 다음 cURL 명령을 실행하여 키에 대한 일반 특성을 보십시오.

    ```cURL
    curl -X GET \
    https://key-protect.us-south.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>' \
    -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    계정에 지정된 Cloud Foundry 조직과 영역 내에서 키에 대한 작업을 수행하려면 `Bluemix-Instance`를 해당 `Bluemix-org` 및 `Bluemix-space` 헤더로 바꾸십시오. [코드 샘플에 대한 {{site.data.keyword.keymanagementserviceshort}} API 참조 문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")를 참조하십시오.](https://console.ng.bluemix.net/apidocs/639){: new_window}.
    {: tip}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.
    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><em>IAM_token</em></td>
        <td>인증 토큰입니다. cURL 요청에 Bearer 값 등 <code>IAM</code> 토큰의 전체 컨텐츠를 포함하십시오.</td>
      </tr>
      <tr>
        <td><em>instance_ID</em></td>
        <td>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. </td>
      </tr>
      <tr>
        <td><em>correlation_ID</em></td>
        <td>선택사항: 트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 2. {{site.data.keyword.keymanagementserviceshort}} API를 통해 키를 보는 데 필요한 변수에 대해 설명합니다.</caption>
    </table>

    성공한 `POST /v2/keys/` 요청은 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 사용 가능한 키의 콜렉션을 리턴합니다.

    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.collection+json",
        "collectionTotal": 2
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Standard key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": true
        },
        {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Root key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": false
        }
      ]
    }
    ```
    {:screen}

### 2단계: ID별로 키 검색
{: #retrieve_key_api}

특정 키에 대한 자세한 정보를 보기 위해 다음 엔드포인트에 대한 후속 `GET` 호출을 작성할 수 있습니다.

```
https://key-protect.us-south.bluemix.net/api/v2/keys<key_ID>
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/keymgmt/keyprotect_authentication.html)
2. 액세스하거나 관리할 키의 ID를 검색하십시오.

    ID 값을 사용하여 키 자료와 같은 키에 대한 자세한 정보에 액세스할 수 있습니다. `GET v2/keys` 요청을 작성하거나 {{site.data.keyword.keymanagementserviceshort}} GUI에 액세스하여 지정된 키에 대한 ID를 검색할 수 있습니다.

3. 다음 cURL 명령을 실행하여 키와 키 자료에 대한 세부사항을 가져오십시오.

    ```cURL
    curl -X GET \
      https://key-protect.us-south.bluemix.net/api/v2/keys<key_ID> \
      -H 'accept: application/vnd.ibm.kms.key+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.

    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><em>IAM_token</em></td>
        <td>인증 토큰입니다. cURL 요청에 Bearer 값 등 `IAM` 토큰의 전체 컨텐츠를 포함하십시오.</td>
      </tr>
      <tr>
        <td><em>instance_ID</em></td>
        <td>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. </td>
      </tr>
      <tr>
        <td><em>correlation_ID</em></td>
        <td>선택사항: 트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><em>key_ID</em></td>
        <td>[1단계](#retrieve_keys_api)에서 검색한 키에 대한 ID입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 3. {{site.data.keyword.keymanagementserviceshort}} API를 통해 지정된 키를 보는 데 필요한 변수에 대해 설명합니다.</caption>
    </table>

    성공한 `GET v2/keys/<key_ID>` 응답은 키 및 키 자료에 대한 세부사항을 리턴합니다. 다음 JSON 오브젝트는 표준 키의 리턴값 예를 표시합니다.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.key+json"
        },
    "resources": [
      {
            "id": "...",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Standard key",
            "description": "...",
            "state": 1,
            "crn": "...",
            "algorithmType": "AES",
            "payload": "...",
            "createdBy": "...",
            "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
            "lastUpdateDate": "YYYY-MM-DDTHH:MM:SSZ",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "GCM"
            },
            "extractable": true
        }
      ]
    }
    ```
    {:screen}

    사용 가능한 매개변수에 대한 자세한 설명은 {{site.data.keyword.keymanagementserviceshort}} [REST API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/apidocs/639){: new_window}를 참조하십시오.
