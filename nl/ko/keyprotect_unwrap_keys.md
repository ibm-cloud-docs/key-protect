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

# 키 랩핑 해제
{: #unwrapping-keys}

권한 있는 사용자인 경우 {{site.data.keyword.keymanagementservicefull}} API를 사용하여 해당 컨텐츠에 액세스하도록 데이터 암호화 키(DEK)를 랩핑 해제할 수 있습니다. DEK를 랩핑 해제하면 해당 컨텐츠의 무결성이 복호화되고 검사되며, 원래 키 자료가 {{site.data.keyword.cloud_notm}} 데이터 서비스에 리턴됩니다.
{: shortdesc}

키 랩핑을 통해 클라우드에서 저장 데이터의 보안을 제어하는 방법을 알아보려면 [엔벨로프 암호화](/docs/services/keymgmt/keyprotect_envelope.html)를 참조하십시오.

## API를 사용하여 키 랩핑 해제
{: #api}

[서비스에 대한 랩핑 호출을 작성하면](/docs/services/keymgmt/keyprotect_wrap_keys.html), 다음 엔드포인트에 대한 `POST` 호출을 작성하여 해당 컨텐츠에 액세스하도록 지정된 데이터 암호화 키(DEK)를 랩핑 해제할 수 있습니다.

```
https://keyprotect.us-south.bluemix.net/api/v2/keys<key_id>?action=unwrap
```
{: codeblock}

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/keymgmt/keyprotect_authentication.html)

2. 초기 랩핑 요청을 수행하는 데 사용한 루트 키의 ID를 복사하십시오.

    `GET /v2/keys/` 요청을 작성하거나 {{site.data.keyword.keymanagementserviceshort}} GUI에서 키를 확인하여 키의 ID를 검색할 수 있습니다.

3. 초기 랩핑 요청 중에 리턴된 `ciphertext` 값을 복사하십시오.

4. 다음 cURL 명령을 실행하여 키 자료를 복호화하고 인증하십시오.

    ```cURL
    curl -X POST \
      'https://keyprotect.us-south.bluemix.net/api/v2/keys<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
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
        <td>랩핑에 사용한 루트 키의 고유 ID입니다. 랩핑 해제 요청이 성공하도록 하려면 초기 랩핑 요청 중에 사용한 동일한 루트 키를 제공하십시오.</td>
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
        <td><em>additional_data</em></td>
        <td>선택사항: 키를 보안하는 데 사용되는 추가 인증 데이터(AAD)입니다. 각 문자열은 최대 255자입니다.<br></br>중요: 서비스에 대한 랩핑 호출을 작성할 때 AAD를 제공한 경우 랩핑 해제 호출 중에 동일한 AAD를 지정해야 합니다.</td>
      </tr>
      <tr>
        <td><em>encrypted_data_key</em></td>
        <td>랩핑 오퍼레이션 중에 리턴된 <code>ciphertext</code> 값입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}}에서 키를 랩핑 해제하는 데 필요한 변수에 대해 설명합니다.</caption>
    </table>

    원래 키 자료는 응답 엔티티-본문에 리턴됩니다. 다음 JSON 오브젝트는 예제 리턴값을 표시합니다.

    ```
    {
      "plaintext": "s~Rz@kN9Fzv\\/hP*r3LY-?O@!!qdtj:L"
    }
    ```
    {:screen}
