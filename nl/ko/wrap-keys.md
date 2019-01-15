---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 키 랩핑
{: #wrap-keys}

권한 있는 사용자인 경우 {{site.data.keyword.keymanagementservicelong}} API를 사용하여 암호화 키를 관리하고 루트 키로 보호할 수 있습니다.
{: shortdesc}

루트 키로 데이터 암호화 키(DEK)를 랩핑하면 {{site.data.keyword.keymanagementserviceshort}}가 여러 알고리즘의 장점을 결합하여 암호화된 데이터의 무결성과 개인정보를 보호합니다.  

키 랩핑을 통해 클라우드에서 저장 데이터의 보안을 제어하는 방법을 알아보려면 [엔벨로프 암호화](/docs/services/key-protect/concepts/envelope-encryption.html)를 참조하십시오.

## API를 사용하여 키 랩핑
{: #api}

{{site.data.keyword.keymanagementserviceshort}}에서 관리하는 루트 키로 지정된 데이터 암호화 키(DEK)를 보호할 수 있습니다.

랩핑을 위해 루트 키를 제공하는 경우 랩핑 호출에 성공할 수 있도록 루트 키가 256, 384 또는 512비트인지 확인하십시오. 서비스에서 루트 키를 작성하는 경우 {{site.data.keyword.keymanagementserviceshort}}가 HSM에서 256비트 키를 생성하며, 이는 AES-GCM 알고리즘에서 지원됩니다.
{: note}

[서비스에서 루트 키를 지정하면](/docs/services/key-protect/create-root-keys.html) 다음 엔드포인트에 대한 `POST` 호출을 작성하여 고급 암호화로 DEK를 랩핑할 수 있습니다.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/key-protect/access-api.html)

2. 관리하고 보호할 DEK의 키 자료를 복사하십시오.

    {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 대한 관리자 또는 작성자 권한이 있으면 [`GET /v2/keys/<key_ID>` 요청을 작성하여 특정 키의 키 자료를 검색할 수 있습니다](/docs/services/key-protect/view-keys.html#api).

3. 랩핑에 사용할 루트 키의 ID를 복사하십시오.

4. 다음 cURL 명령을 실행하여 랩핑 오퍼레이션으로 키를 보호하십시오.

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    계정에서 Cloud Foundry 조직과 영역 내의 키에 대한 작업을 수행하려면 `Bluemix-Instance`를 적절한 `Bluemix-org` 및 `Bluemix-space` 헤더로 바꾸십시오. [자세한 정보는 {{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 참조하십시오.
    {: tip}

    다음 표에 따라 예제 요청의 변수를 대체하십시오.

    <table>
      <tr>
        <th>변수</th>
        <th>설명</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스가 상주하는 지리적 영역을 표시하는 지역 약어(예: <code>us-south</code> 또는 <code>eu-gb</code>)입니다. 자세한 정보는 <a href="/docs/services/key-protect/regions.html#endpoints">지역 서비스 엔드포인트</a>를 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>필수.</strong> 랩핑에 사용할 루트 키의 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>필수.</strong> 사용자의 {{site.data.keyword.cloud_notm}} 액세스 토큰입니다. cURL 요청에 Bearer 값 등 <code>IAM</code> 토큰의 전체 컨텐츠를 포함하십시오. 자세한 정보는 <a href="/docs/services/key-protect/access-api.html#retrieve-token">액세스 토큰 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>필수.</strong> {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 지정된 고유 ID입니다. 자세한 정보는 <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">인스턴스 ID 검색</a>을 참조하십시오.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>트랜잭션을 추적하고 상관시키는 데 사용되는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><varname>data_key</varname></td>
        <td>관리하고 보호할 DEK의 키 자료입니다. <code>plaintext</code> 값은 base64로 인코딩되어야 합니다. 새 DEK를 생성하려면 <code>plaintext</code> 속성을 생략하십시오. 서비스는 랜덤 일반 텍스트(32바이트)를 생성하고 이 값을 랩핑한 후 응답에 생성되고 랩핑된 값을 모두 리턴합니다.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>키를 보안하는 데 사용되는 추가 인증 데이터(AAD)입니다. 각 문자열은 최대 255자입니다. 서비스에 대한 랩핑 호출을 작성할 때 AAD를 제공한 경우 후속 랩핑 해제 호출 중에 동일한 AAD를 지정해야 합니다.<br></br>중요사항: {{site.data.keyword.keymanagementserviceshort}} 서비스는 추가 인증 데이터를 저장하지 않습니다. AAD를 제공하는 경우에는 후속 랩핑 해제 요청 중에 동일한 AAD를 액세스 및 제공할 수 있도록 안전한 위치에 데이터를 저장하십시오.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}}에서 지정된 키를 랩핑하는 데 필요한 변수에 대한 설명</caption>
    </table>

    base64로 인코딩된 키 자료가 포함된 랩핑된 데이터 암호화 키는 응답 엔티티-본문에 리턴됩니다. 다음 JSON 오브젝트는 예제 리턴값을 표시합니다.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    랩핑 요청을 수행할 때 `plaintext` 속성을 생략하면 서비스는 base64로 인코딩된 형식으로 생성된 데이터 암호화 키(DEK) 및 랩핑된 DEK를 모두 리턴합니다.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    <code>plaintext</code> 값은 랩핑 해제된 DEK를 나타내고 <code>ciphertext</code> 값은 랩핑된 DEK를 나타냅니다.
    
    사용자 대신 {{site.data.keyword.keymanagementserviceshort}}에서 새 데이터 암호화 키(DEK)를 생성하려면 랩핑 요청에 따라 비어 있는 본문도 전달할 수 있습니다. base64로 인코딩된 키 자료가 포함된 생성된 DEK는 랩핑된 DEK와 함께 응답 엔티티-본문에 리턴됩니다.
    {: tip}
    
