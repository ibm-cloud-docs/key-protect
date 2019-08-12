---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: key management service, kms, manage encryption keys, data encryption, data-at-rest, protect data encryption keys

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 시작하기 튜토리얼
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}}를 사용하면 {{site.data.keyword.cloud_notm}} 서비스 간에 앱에 대한 암호화된 키를 프로비저닝하는 데 도움이 됩니다. 이 튜토리얼에는 {{site.data.keyword.keymanagementserviceshort}} 대시보드를 사용하여 기존 암호화 키를 작성하고 추가하는 방법이 표시되어 있으므로, 하나의 중심 위치에서 데이터 암호화를 관리할 수 있습니다.
{: shortdesc}

## 암호화 키 시작하기
{: #get-started-keys}

{{site.data.keyword.keymanagementserviceshort}} 대시보드에서 암호화를 위한 새 키를 작성하거나 기존 키를 가져올 수 있습니다. 

다음 두 키 유형에서 선택하십시오.

<dl>
  <dt>루트 키</dt>
    <dd>루트 키는 {{site.data.keyword.keymanagementserviceshort}}에서 완전히 관리하는 대칭 키-랩핑 키입니다. 루트 키를 사용하여 고급 암호화를 통해 다른 암호화 키를 보호할 수 있습니다. 자세한 내용은 <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">엔벨로프 암호화로 데이터 보호</a>를 참조하십시오.</dd>
  <dt>표준 키</dt>
    <dd>표준 키는 암호화에 사용되는 대칭 키입니다. 표준 키를 사용하여 데이터를 직접 암호화하고 복호화할 수 있습니다.</dd>
</dl>

## 키 새로 작성
{: #create-keys}

[{{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 작성하고 나면](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps) 서비스에서 키를 지정할 준비가 된 것입니다. 

다음 단계를 완료하여 첫 번째 암호화 키를 작성하십시오. 

1. 애플리케이션 세부사항 페이지에서 **관리** &gt; **키 추가**를 클릭하십시오.
2. 키를 새로 작성하려면 **키 생성** 창을 선택하십시오.

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
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">키의 유형</a>입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. <b>키 작성</b> 설정에 대한 설명</caption>
    </table>

3. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 작성**을 클릭하십시오. 

서비스에서 작성되는 키는 대칭 256비트 키이며, AES-CBC-PAD 알고리즘에 의해 지원됩니다. 보안 강화를 위해 키는 보안 {{site.data.keyword.cloud_notm}} 데이터 센터에 있는 FIPS 140-2 레벨 3 공인 HSM(Hardware Security Module)에 의해 생성됩니다. 

## 고유 키 가져오기
{: #import-keys}

서비스에 기존 키를 도입하여 BYOK(Bring Your Own Key)의 보안 이점을 사용할 수 있습니다. 

다음 단계를 완료하여 기존 키를 추가하십시오.

1. 애플리케이션 세부사항 페이지에서 **관리** &gt; **키 추가**를 클릭하십시오.
2. 기존 키를 업로드하려면 **고유 키 가져오기** 창을 선택하십시오.

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
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">키의 유형</a>입니다.</td>
      </tr>
      <tr>
        <td>키 자료</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} 서비스에 저장할 키 자료입니다(예: 대칭 키). 제공하는 키는 base64로 인코딩되어야 합니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 2. <b>고유 키 가져오기</b> 설정에 대한 설명</caption>
    </table>

3. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 가져오기**를 클릭하십시오. 

{{site.data.keyword.keymanagementserviceshort}} 대시보드에서 새 키의 일반 특성을 검사할 수 있습니다. 

## 다음에 수행할 작업
{: #get-started-next-steps}

이제 키를 사용하여 앱과 서비스를 코딩할 수 있습니다. 서비스에 루트 키를 추가한 경우 루트 키를 사용하여 저장 데이터를 암호화하는 키를 보호하는 방법에 대해 자세히 알아보십시오. 시작하려면 [키 랩핑](/docs/services/key-protect?topic=key-protect-wrap-keys)을 확인하십시오.

- 루트 키로 암호화 키 관리 및 보호에 대해 자세히 알아보려면 [엔벨로프 암호화로 데이터 보호](/docs/services/key-protect?topic=key-protect-envelope-encryption)를 참조하십시오.
- {{site.data.keyword.keymanagementserviceshort}} 서비스와 다른 클라우드 데이터 솔루션과의 통합에 대해 자세히 알아보려면 [통합 문서를 확인](/docs/services/key-protect?topic=key-protect-integrate-services)하십시오.
- 프로그래밍 방식의 키 관리에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서를 확인하십시오](https://cloud.ibm.com/apidocs/key-protect){: external}.
