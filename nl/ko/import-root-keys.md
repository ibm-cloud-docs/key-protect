---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# 루트 키 가져오기
{: #import-root-keys}

{{site.data.keyword.keymanagementservicefull}}에서는 {{site.data.keyword.keymanagementserviceshort}} GUI를 사용하거나 프로그래밍 방식으로 {{site.data.keyword.keymanagementserviceshort}} API를 사용하여 기존 루트 키를 보안할 수 있습니다.
{: shortdesc}

루트 키는 클라우드에서 암호화된 데이터의 보안을 보호하는 데 사용되는 대칭 키-랩핑 키입니다. {{site.data.keyword.keymanagementserviceshort}}로 루트 키를 가져오는 데 대한 자세한 정보는 [클라우드로 암호화 키 가져오기](/docs/services/key-protect?topic=key-protect-importing-keys)를 참조하십시오.

## GUI를 사용하여 루트 키 가져오기
{: #import-root-key-gui}

[서비스의 인스턴스를 작성한 후](/docs/services/key-protect?topic=key-protect-provision), 다음 단계를 완료하여 {{site.data.keyword.keymanagementserviceshort}} GUI로 기존 루트 키를 추가하십시오.

1. [{{site.data.keyword.cloud_notm}} 콘솔 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")에 로그인](https://{DomainName}/){: new_window}하십시오.
2. **메뉴** &gt; **리소스 목록**으로 이동하여 리소스 목록을 보십시오.
3. {{site.data.keyword.cloud_notm}} 리소스 목록에서 {{site.data.keyword.keymanagementserviceshort}}의 프로비저닝된 인스턴스를 선택하십시오.
4. 키를 가져오려면 **키 추가**를 클릭하고 **고유 키 가져오기** 창을 선택하십시오.

    키의 세부사항을 지정하십시오.

    <table>
      <tr>
        <th>설정</th>
        <th>설명</th>
      </tr>
      <tr>
        <td>이름</td>
        <td>
          <p>키를 쉽게 식별할 수 있도록 해 주는 사용자가 읽을 수 있는 고유 별명입니다.</p>
          <p>개인정보를 보호하려면 키 이름에 사용자 이름 또는 위치와 같은 PII(Personally Identifiable Information)가 포함되지 않았는지 확인하십시오.</p>
        </td>
      </tr>
      <tr>
        <td>키 유형</td>
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">키의 유형</a>입니다. 키 유형 목록에서 <b>루트 키</b>를 선택하십시오.</td>
      </tr>
      <tr>
        <td>키 자료</td>
        <td>
          <p>서비스에 저장하고 관리할 base64로 인코딩된 키 자료입니다(예: 기존 키-랩핑 키).</p>
          <p>키 자료가 다음 요구사항을 충족시키는지 확인하십시오.</p>
          <p>
            <ul>
              <li>키는 128, 192 또는 256비트여야 합니다.</li>
              <li>base64 인코딩을 사용하여 데이터 바이트(예: 256비트의 경우 32바이트)를 인코딩해야 합니다.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">표 1. <b>고유 키 가져오기</b> 설정에 대한 설명</caption>
    </table>

5. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 가져오기**를 클릭하십시오. 

## API를 사용하여 루트 키 가져오기
{: #import-root-key-api}

{{site.data.keyword.keymanagementserviceshort}} API를 사용하여 루트 키를 서비스로 가져올 수 있습니다.

[키 자료 작성 및 암호화를 위한 옵션을 검토](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)하여 키 가져오기를 계획하십시오. 보안을 추가하려면 클라우드로 키 자료를 가져오기 전에 이 키 자료를 암호화도록 [전송 키](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys)를 사용하여 키 자료의 보안 가져오기를 사용하도록 설정할 수 있습니다. 전송 키를 사용하지 않고 루트 키를 가져오려면 [4단계](#import-root-key)로 건너뛰십시오.
{: note}

### 1단계: 전송 키 작성
{: #create-transport-key}

전송 키는 현재 베타 기능입니다. 베타 기능은 언제든지 변경할 수 있으며 향후 업데이트는 최신 버전과 호환되지 않는 변경사항을 도입할 수 있습니다.
{: important}

다음 엔드포인트에 대한 `POST` 호출을 작성하여 서비스 인스턴스의 전송 키를 작성하십시오.

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
      <td>더 이상 액세스할 수 없게 될 때까지 만기 시간 내에 전송 키를 검색할 수 있는 횟수입니다. 기본값은 1입니다.</td>
    </tr>
      <caption style="caption-side:bottom;">표 2. 전송 키 {{site.data.keyword.keymanagementserviceshort}} API 작성에 필요한 변수에 대한 설명</caption>
  </table>

  성공한 `POST api/v2/lockers` 응답은 기타 메타데이터와 함께 전송 키의 ID 값을 리턴합니다. ID는 전송 키와 연관되며 {{site.data.keyword.keymanagementserviceshort}} API에 대한 후속 호출에 사용되는 고유 ID입니다.

### 2단계: 전송 키 검색 및 토큰 가져오기
{: #retrieve-transport-key}

다음 엔드포인트에 대한 `GET` 호출을 작성하여 전송 키 및 가져오기 토큰을 검색하십시오.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. 다음 cURL 명령을 사용하여 [{{site.data.keyword.keymanagementserviceshort}} API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 호출하십시오.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>locker_ID</varname></td>
        <td><strong>필수.</strong> <a href="#create-transport-key">1단계</a>에서 작성한 전송 키에 대한 ID입니다.</td>
      </tr>
        <caption style="caption-side:bottom;">표 3. {{site.data.keyword.keymanagementserviceshort}} API로 전송 키를 검색하는 데 필요한 변수에 대한 설명</caption>
    </table>

    성공한 `GET api/v2/lockers/{id}` 응답은 전송 키의 무결성을 확인하는 데 사용되는 가져오기 토큰과 함께 루트 키 자료를 암호화하는 데 사용할 수 있는 PKIX 형식의 4096비트 DER 인코딩 공용 암호화 키를 리턴합니다.

### 3단계: 키 자료 암호화
{: #encrypt-root-key}

전송 키를 검색한 후 키를 사용하여 {{site.data.keyword.keymanagementserviceshort}}로 가져올 키 자료를 암호화하십시오.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

온프레미스에서 키 자료를 생성하려면 [대칭 암호화 키 작성을 위한 옵션을 검토](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead)하십시오. 예를 들어, 온프레미스 HSM(Hardware Security Module)에서 지원하는 조직의 내부 키 관리 시스템을 사용하여 키 자료를 작성하고 내보낼 수 있습니다.
{: note}

키 자료를 암호화하려면 다음을 수행하십시오.

1. 온프레미스 키 관리 시스템에서 바이너리 형식으로 256비트 키 자료를 내보내십시오.

    키 자료를 작성하고 내보내는 방법에 대해 알아보려면 온프레미스 HSM 또는 키 관리 시스템에 대한 문서를 참조하십시오.

2. 2단계의 [검색된 전송 키](#retrieve-transport-key)를 사용하여 키 자료를 암호화하십시오.

   키 자료를 암호화할 때 `RSAES_OAEP_SHA_256` 암호화 스킴을 사용하십시오. 이는 {{site.data.keyword.keymanagementserviceshort}}가 전송 키 작성에 사용하는 기본 스킴입니다. {{site.data.keyword.keymanagementserviceshort}}에서 복호화 문제를 방지하려면 키 자료에 대해 RSAES_OAEP 암호화를 실행할 때 선택적 `label` 매개변수를 포함하지 마십시오. 키 자료에 대해 RSA 암호화를 실행하는 방법에 대해 알아보려면 온프레미스 HSM 또는 키 관리 시스템에 대한 문서를 참조하십시오.

3. 다음 단계를 계속하기 전에 암호화된 키 자료가 base64로 인코딩되었는지 확인하십시오.

### 4단계: 키 자료 가져오기
{: #import-root-key}

[키 자료를 암호화하고 base64로 인코딩한 후](#encrypt-root-key), 다음 엔드포인트에 대한 `POST` 호출을 작성하여 루트 키를 {{site.data.keyword.keymanagementserviceshort}}로 가져오십시오.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. 다음 cURL 명령을 사용하여 [{{site.data.keyword.keymanagementserviceshort}} API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 호출하십시오.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
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
       "payload": "<key_material>",
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
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
        <td><varname>key_alias</varname></td>
        <td><strong>필수.</strong> 키를 쉽게 식별할 수 있도록 해 주는 사용자가 읽을 수 있는 고유 이름입니다. 개인정보를 보호하려면 개인 데이터를 키의 메타데이터로 저장하지 마십시오.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>키에 대한 자세한 설명입니다. 개인정보를 보호하려면 개인 데이터를 키의 메타데이터로 저장하지 마십시오.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>시스템에서 키가 만료되는 날짜 및 시간입니다(RFC 3339 형식). <code>expirationDate</code> 속성이 생략되면 키가 만료되지 않습니다.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>서비스에 저장하고 관리할 base64로 인코딩된 키 자료입니다(예: 기존 키-랩핑 키).</p>
          <p>키 자료가 다음 요구사항을 충족시키는지 확인하십시오.</p>
          <p>
            <ul>
              <li>키는 128, 192 또는 256비트여야 합니다.</li>
              <li>base64 인코딩을 사용하여 데이터 바이트(예: 256비트의 경우 32바이트)를 인코딩해야 합니다.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>키 자료 서비스를 중단할지 여부를 판별하는 부울 값입니다.</p>
          <p><code>extractable</code> 속성을 <code>false</code>로 설정하면 서비스가 키를 <code>wrap</code> 또는 <code>unwrap</code> 오퍼레이션에 사용할 수 있는 루트 키로 지정합니다.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td><a href="#encrypt-root-key">키 자료 암호화</a>에 사용한 암호화 스킴입니다. 현재 <code>RSAES_OAEP_SHA_256</code>이 지원됩니다. 전송 키와 가져오기 토큰을 사용하지 않고 루트 키 자료를 가져오려면 <code>encryption_algorithm</code> 속성을 생략하십시오.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>전송 키의 활성 및 무결성 확인에 사용되는 가져오기 토큰입니다. 전송 키를 사용하여 키 자료를 암호화하는 경우 <a href="#retrieve-transport-key">2단계</a>에서 검색한 동일한 가져오기 토큰을 제공해야 합니다. 전송 키와 가져오기 토큰을 사용하지 않고 루트 키 자료를 가져오려면 <code>importToken</code> 속성을 생략하십시오.</td>
      </tr>
        <caption style="caption-side:bottom;">표 4. {{site.data.keyword.keymanagementserviceshort}} API를 통해 루트 키를 추가하는 데 필요한 변수에 대한 설명</caption>
    </table>

    개인 데이터의 기밀성을 보호하려면 서비스에 키를 추가할 때 사용자 이름 또는 위치와 같은 PII(Personally Identifiable Information)를 입력하지 않도록 하십시오. PII에 대한 추가 예제는 [NIST Special Publication 800-122 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}의 섹션 2.2를 참조하십시오.
    {: important}

    성공한 `POST api/v2/keys` 응답은 기타 메타데이터와 함께 키의 ID 값을 리턴합니다. ID는 키에 지정되어 있으며 {{site.data.keyword.keymanagementserviceshort}} API에 대한 후속 호출에 사용되는 고유 ID입니다.

2. 선택사항: {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 키를 찾아보는 다음 호출을 실행하여 키가 추가되었는지 확인하십시오.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## 다음에 수행할 작업
{: #import-root-key-next-steps}

- 엔벨로프 암호화로 키를 보호하는 데 대해 자세히 알아보려면 [키 랩핑](/docs/services/key-protect?topic=key-protect-wrap-keys)을 확인하십시오.
- 프로그래밍 방식의 키 관리에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 확인하십시오.
