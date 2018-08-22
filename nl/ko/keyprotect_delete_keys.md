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

# 키 삭제
{: #deleting-keys}

{{site.data.keyword.cloud_notm}} 영역 또는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스의 관리자인 경우에는 {{site.data.keyword.keymanagementservicefull}}를 사용하여 암호화 키와 해당 컨텐츠를 삭제할 수 있습니다.
{: shortdesc}

**중요:** 키를 삭제하면 해당 컨텐츠 및 연관된 데이터가 영구 삭제됩니다. 이 조치는 되돌릴 수 없습니다. 프로덕션 환경의 경우에는 리소스 영구 삭제가 권장되지 않지만, 테스트나 QA 등의 임시 환경의 경우에는 유용할 수 있습니다.

## GUI로 키 삭제
{: #gui}

그래픽 인터페이스를 사용한 암호화 키 삭제를 원하는 경우 {{site.data.keyword.keymanagementserviceshort}} GUI를 사용할 수 있습니다.

[키를 새로 작성하거나 기존 키를 서비스로 가져온 후](/docs/services/keymgmt/keyprotect_create_keys.html) 다음 단계를 완료하여 키를 삭제하십시오.

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/)에 로그인하십시오.
2. {{site.data.keyword.cloud_notm}} 대시보드에서 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스를 선택하십시오.
3. **키** 테이블을 사용하여 서비스에서 키를 찾아보십시오.
4. 아이콘을 클릭하여 삭제할 키에 대한 옵션 목록을 여십시오.
5. 옵션 메뉴에서 **키 삭제**를 클릭한 후 다음 화면에서 키 삭제를 확인하십시오.

키를 삭제하면 키가 _영구 삭제됨_ 상태로 전이됩니다. 이 상태의 키는 더 이상 복구 불가능합니다. 키와 연관된 메타데이터(예: 키의 삭제 날짜)가 {{site.data.keyword.keymanagementserviceshort}} 데이터베이스에 보관됩니다.

## API로 키 삭제
{: #api}

키와 해당 컨텐츠를 삭제하려면 다음 엔드포인트에 대해 `DELETE` 호출을 작성하십시오.

```
https://keyprotect.us-south.bluemix.net/api/v2/keys<key_ID>
```

1. [서비스 및 인증 신임 정보를 검색하여 서비스에서 키에 대한 작업을 수행하십시오.](/docs/services/keymgmt/keyprotect_authentication.html)

2. 삭제할 키의 ID를 검색하십시오.

    `GET /v2/keys/` 요청을 작성하거나 {{site.data.keyword.keymanagementserviceshort}} 대시보드에서 키를 확인하여 지정된 키의 ID를 검색할 수 있습니다.

3. 다음 cURL 명령을 실행하여 키와 해당 컨텐츠를 영구 삭제하십시오.

    ```cURL
    curl -X DELETE \
      https://keyprotect.us-south.bluemix.net/api/v2/keys<key_ID> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'prefer: <return_preference>'
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
        <td><em>key_ID</em></td>
        <td>삭제할 키의 고유 ID입니다.</td>
      </tr>
      <tr>
      <tr>
        <td><em>return_preference</em></td>
        <td><p><code>POST</code> 및 <code>DELETE</code> 오퍼레이션에 대한 서버 작동을 변경하는 헤더입니다.</p><p><em>return_preference</em> 변수를 <code>return=minimal</code>로 설정하면 서비스가 성공한 삭제 응답을 리턴합니다. 변수를 <code>return=representation</code>으로 설정하면 서비스가 키 자료와 키 메타데이터를 둘 다 리턴합니다.</p></td>
      </tr>
      <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 키를 삭제하는 데 필요한 변수를 설명합니다.</caption>
    </table>

    _return_preference_ 변수가 `return=representation`으로 설정되면 `DELETE` 요청의 세부사항이 응답 엔티티-본문에 리턴됩니다. <!--After you delete a key, it enters the `Deactivated` key state. After 24 hours, if a key is not reinstated, the key transitions to the `Destroyed` state. The key contents are permanently erased and no longer accessible.--> 다음 JSON 오브젝트는 예제 리턴값을 표시합니다.
    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "...",
          "description": "...",
          "state": 5,
          "crn": "...",
          "deleted": true,
          "algorithmType": "AES",
          "createdBy": "...",
          "deletedBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "deletionDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "lastUpdateDate": "YYYY-MM-DDTHH:MM:SS.SSZ",
          "nonactiveStateReason": 2,
          "extractable": true
        }
      ]
    }
    ```
    {: screen}

    사용 가능한 매개변수에 대한 자세한 설명은 {{site.data.keyword.keymanagementserviceshort}} [REST API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/apidocs/639){: new_window}를 참조하십시오.
