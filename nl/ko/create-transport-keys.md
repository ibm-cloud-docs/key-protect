---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# 전송 키 작성
{: #create-transport-keys}

먼저 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스의 전송 암호화 키를 작성하여 클라우드로 루트 키 자료의 보안 가져오기를 사용하도록 설정할 수 있습니다.
{: shortdesc}

전송 키는 지정한 정책에 따라 {{site.data.keyword.keymanagementserviceshort}}로 루트 키 자료를 암호화하고 안전하게 가져오는 데 사용됩니다. 클라우드로 키를 안전하게 가져오는 데 대해 자세히 알아보려면 [클라우드로 암호화 키 가져오기](/docs/services/key-protect/concepts?topic=key-protect-importing-keys)를 참조하십시오.

전송 키는 현재 베타 기능입니다. 베타 기능은 언제든지 변경할 수 있으며 향후 업데이트는 최신 버전과 호환되지 않는 변경사항을 도입할 수 있습니다.
{: important}

## API로 전송 키 작성
{: #create-transport-key-api}

다음 엔드포인트에 대한 `POST` 호출을 작성하여 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스와 연관된 전송 키를 작성하십시오.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [서비스 및 인증용 인증 정보를 검색하여 서비스에서 키에 대한 작업을 수행](/docs/services/key-protect?topic=key-protect-set-up-api)하십시오.

2. [{{site.data.keyword.keymanagementserviceshort}} API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 호출하여 전송 키에 대한 정책을 설정하십시오.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
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
          <td><varname>expiration_time</varname></td>
          <td>
            <p>키가 유효하게 유지되는 기간을 판별하는 전송 키 작성 시점부터의 시간(초)입니다.</p>
            <p>최소값은 300초(5분)이며 최대값은 86400초(24시간)입니다. 기본값은 600초(10분)입니다.</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>더 이상 액세스할 수 없게 될 때까지 만기 시간 내에 전송 키를 검색할 수 있는 횟수입니다. 기본값은 1입니다. </td>
        </tr>
          <caption style="caption-side:bottom;">표 1. {{site.data.keyword.keymanagementserviceshort}} API를 통해 루트 키를 추가하는 데 필요한 변수에 대한 설명</caption>
      </table>

    성공한 `POST api/v2/lockers` 요청은 서비스 인스턴스의 전송 키를 작성하고 다른 메타데이터와 함께 해당 ID 값을 리턴합니다. ID는 전송 키와 연관되며 {{site.data.keyword.keymanagementserviceshort}} API에 대한 후속 호출에 사용되는 고유 ID입니다.

3. 선택사항: 서비스 인스턴스에 대한 메타데이터를 검색하기 위해 다음 호출을 실행하여 전송 키를 작성했는지 확인하십시오.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 다음에 수행할 작업
{: #create-transport-key-next-steps}

- 전송 키를 사용하여 루트 키를 서비스로 가져오는 데 대해 자세히 알아보려면 [루트 키 가져오기](/docs/services/key-protect?topic=key-protect-import-root-keys)를 참조하십시오.
- 프로그래밍 방식의 키 관리에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 확인하십시오.
