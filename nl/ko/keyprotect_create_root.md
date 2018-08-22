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

# 루트 키 작성
{: #create_root_keys}

{{site.data.keyword.keymanagementservicefull}}에서는 {{site.data.keyword.keymanagementserviceshort}} GUI를 사용하거나 프로그래밍 방식으로 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 루트 키를 작성할 수 있습니다.
{: shortdesc}

루트 키는 클라우드에서 암호화된 데이터의 보안을 보호하는 데 사용되는 대칭 키-랩핑 키입니다. 루트 키에 대한 자세한 정보는 [엔벨로프 암호화](/docs/services/keymgmt/keyprotect_envelope.html)를 참조하십시오. 

## GUI로 루트 키 작성
{: #create_root_key_GUI}

[서비스의 인스턴스를 작성한 후](/docs/services/keymgmt/keyprotect_provision.html), 다음 단계를 완료하여 {{site.data.keyword.keymanagementserviceshort}} GUI로 루트 키를 작성하십시오.

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/)에 로그인하십시오.
2. {{site.data.keyword.cloud_notm}} 대시보드에서 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스를 선택하십시오.
2. 키를 새로 작성하려면 **키 추가**를 클릭하고 **새 키 생성** 창을 선택하십시오.

    키의 세부사항을 지정하십시오.

    <table>
      <tr>
        <th>설정</th>
        <th>설명</th>
      </tr>
      <tr>
        <td>이름</td>
        <td>키에 지정할, 사용자가 읽을 수 있는 고유한 별명입니다. 키의 용도 또는 키와 연관된 사용자와 같이, 키를 식별하는 데 도움이 되는 이름을 지정할 수 있습니다.</td>
      </tr>
      <tr>
        <td>키 유형</td>
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 [키의 유형](/docs/services/keymgmt/keyprotect_envelope.html#key_types)입니다. 키 유형 목록에서 <b>루트 키</b>를 선택하십시오.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. <b>새 키 생성</b> 설정에 대한 설명</caption>
    </table>

3. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 생성**을 클릭하십시오. 

## API로 루트 키 작성
{: #create_root_key_API}

다음 엔드포인트에 대한 `POST` 호출을 작성하여 루트 키를 작성하십시오.

```
https://keyprotect.us-south.bluemix.net/api/v2/keys
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오](/docs/services/keymgmt/keyprotect_authentication.html).

2. 다음 cURL 명령을 사용하여 [{{site.data.keyword.keymanagementserviceshort}}API](https://console.ng.bluemix.net/apidocs/639)를 호출하십시오.

    ```cURL
    curl -X POST \
      https://keyprotect.us-south.bluemix.net/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "extractable": false
       }
     ]
    }'
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
        <td>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><em>correlation_ID</em></td>
        <td>트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><em>key_alias</em></td>
        <td>키를 쉽게 식별할 수 있도록 해 주는 사용자가 읽을 수 있는 고유 이름입니다.</td>
      </tr>
      <tr>
        <td><em>key_description</em></td>
        <td>키의 자세한 설명입니다.</td>
      </tr>
      <tr>
        <td><em>YYYY-MM-DD</em><br><em>HH:MM:SS.SS</em></td>
        <td>선택사항: 시스템에서 키가 만료되는 날짜 및 시간입니다(RFC 3339 형식). <code>expirationDate</code> 속성이 생략되면 키가 만료되지 않습니다. </td>
      </tr>
      <tr>
        <td><em>key_type</em></td>
        <td>
          <p>키 자료 서비스를 중단할지 여부를 판별하는 부울 값입니다.</p>
          <p><code>extractable</code> 속성을 <code>false</code>로 설정하면 서비스가 <code>wrap</code> 또는 <code>unwrap</code> 오퍼레이션에 사용할 수 있는 루트 키를 작성합니다.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 루트 키를 추가하는 데 필요한 변수</caption>
    </table>

    성공한 `POST /v2/keys/` 응답은 다른 메타데이터와 함께 키의 ID 값을 리턴합니다. ID는 키에 지정되어 있으며 {{site.data.keyword.keymanagementserviceshort}} API에 대한 후속 호출에 사용되는 고유 ID입니다.

3. 선택사항: {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 키를 찾아보는 다음 호출을 실행하여 키가 작성되었는지 확인하십시오.

    ```cURL
    curl -X GET \
      https://keyprotect.us-south.bluemix.net/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

**참고:** 서비스에서 루트 키를 작성하면 키가 {{site.data.keyword.keymanagementserviceshort}} 경계 내에 있으며 해당 키 자료를 검색할 수 없습니다. 

### 다음에 수행할 작업

- 엔벨로프 암호화로 키를 보호하는 데 대해 자세히 알아보려면 [키 랩핑](/docs/services/keymgmt/keyprotect_wrap_keys.html)을 확인하십시오.
- 프로그래밍 방식의 키 관리에 대한 자세한 정보를 찾으려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서에서 코드 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/apidocs/639){: new_window}을 확인하십시오.
