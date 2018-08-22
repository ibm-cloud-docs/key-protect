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

# 키 랩핑
{: #wrapping-keys}

권한 있는 사용자인 경우 {{site.data.keyword.keymanagementservicelong}} API를 사용하여 암호화 키를 관리하고 루트 키로 보호할 수 있습니다.
{: shortdesc}

루트 키로 데이터 암호화 키(DEK)를 랩핑하면 {{site.data.keyword.keymanagementserviceshort}}가 여러 알고리즘의 장점을 결합하여 암호화된 데이터의 무결성과 개인정보를 보호합니다.  

키 랩핑을 통해 클라우드에서 저장 데이터의 보안을 제어하는 방법을 알아보려면 [엔벨로프 암호화](/docs/services/keymgmt/keyprotect_envelope.html)를 참조하십시오.

## API를 사용하여 키 랩핑
{: #api}

{{site.data.keyword.keymanagementserviceshort}}에서 관리하는 루트 키로 지정된 데이터 암호화 키(DEK)를 보호할 수 있습니다.

**중요:** 랩핑을 위해 루트 키를 제공하는 경우 랩핑 호출에 성공할 수 있도록 루트 키가 256, 384 또는 512비트인지 확인하십시오. 서비스에서 루트 키를 작성하는 경우 {{site.data.keyword.keymanagementserviceshort}}가 HSM에서 256비트 키를 생성하며, 이는 AES-GCM 알고리즘에서 지원됩니다.

[서비스에서 루트 키를 지정하면](/docs/services/keymgmt/keyprotect_create_keys.html), 다음 엔드포인트에 대한 `POST` 호출을 작성하여 고급 암호화로 DEK를 랩핑할 수 있습니다.

```
https://keyprotect.us-south.bluemix.net/api/v2/keys<key_ID>?action=wrap
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/keymgmt/keyprotect_authentication.html)

2. 관리하고 보호할 DEK의 키 자료를 복사하십시오.

    {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 대한 관리자 또는 편집자 권한을 가진 경우, [`GET /v2/keys/<key_ID>` 요청을 작성하여 특정 키의 키 자료를 검색할 수 있습니다](/docs/services/keymgmt/keyprotect_view_keys.md#retrieve_key_api).

3. 랩핑에 사용할 루트 키의 ID를 복사하십시오.

4. 다음 cURL 명령을 실행하여 랩핑 오퍼레이션으로 키를 보호하십시오.

    ```cURL
    curl -X POST \
      'https://keyprotect.us-south.bluemix.net/api/v2/keys<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
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
        <td><em>key_ID</em></td>
        <td>랩핑에 사용할 루트 키의 고유 ID입니다. 랩핑 호출에 성공할 수 있도록 루트 키가 256, 384 또는 512비트여야 합니다.</td>
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
      <tr>
        <td><em>return_preference</em></td>
        <td><p><code>POST</code> 및 <code>DELETE</code> 오퍼레이션에 대한 서버 작동을 변경하는 헤더입니다.</p><p><em>return_preference</em> 변수를 <code>return=minimal</code>로 설정하면 서비스가 응답 엔티티-본문에 키 이름 및 ID 값과 같은 키 메타데이터만 리턴합니다. 변수를 <code>return=representation</code>으로 설정하면 서비스가 키 자료와 키 메타데이터를 둘 다 리턴합니다.</p></td>
      </tr>
      <tr>
        <td><em>data_key</em></td>
        <td>선택사항: 관리하고 보호할 DEK의 키 자료입니다. <code>plaintext</code> 값은 base64로 인코딩되어야 합니다. 새 DEK를 생성하려면 <code>plaintext</code> 속성을 생략하십시오. 서비스는 랜덤 일반 텍스트(32비트)를 생성하고 이 값을 랩핑합니다.</td>
      </tr>
      <tr>
        <td><em>additional_data</em></td>
        <td>선택사항: 키를 보안하는 데 사용되는 추가 인증 데이터(AAD)입니다. 각 문자열은 최대 255자입니다. 서비스에 대한 랩핑 호출을 작성할 때 AAD를 제공한 경우 후속 랩핑 해제 호출 중에 동일한 AAD를 지정해야 합니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}}에서 지정된 키를 랩핑하는 데 필요한 변수에 대해 설명합니다.</caption>
    </table>

    base64 인코딩 키 자료가 포함된 랩핑된 키는 응답 엔티티-본문에 리턴됩니다. 다음 JSON 오브젝트는 예제 리턴값을 표시합니다.

    ```
    {
      "plaintext": "s~Rz@kN9Fzv\\/hP*r3LY-?O@!!qdtj:L",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
      "aad": ["data1", "data2"]
    }
    ```
    {:screen}
