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

# {{site.data.keyword.keymanagementserviceshort}} 시작하기

{{site.data.keyword.keymanagementservicefull}}를 사용하면 {{site.data.keyword.cloud_notm}} 서비스 간에 앱에 대한 암호화된 키를 프로비저닝하는 데 도움이 됩니다. 이 튜토리얼에는 {{site.data.keyword.keymanagementserviceshort}} 대시보드를 사용하여 기존 암호화 키를 작성하고 추가하는 방법이 표시되어 있으므로, 하나의 중심 위치에서 데이터 암호화를 관리할 수 있습니다.
{: shortdesc}

## 암호화 키 시작하기
{: #get_started_keys}

{{site.data.keyword.keymanagementserviceshort}} 대시보드에서 암호화를 위한 새 키를 작성하거나 기존 키를 가져올 수 있습니다. 

다음 두 키 유형에서 선택하십시오.

<dl>
  <dt>루트 키</dt>
    <dd>루트 키는 {{site.data.keyword.keymanagementserviceshort}}에서 완전히 관리하는 대칭 키-랩핑 키입니다. 루트 키를 사용하여 고급 암호화를 통해 다른 암호화 키를 보호할 수 있습니다.</dd>
  <dt>표준 키</dt>
    <dd>표준 키는 암호화에 사용되는 대칭 키입니다. 표준 키를 사용하여 데이터를 직접 암호화하고 복호화할 수 있습니다.</dd>
</dl>

## 키 새로 작성
{: #creating_keys}

[{{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 작성한 후](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps) 서비스에서 키를 지정할 준비가 되었습니다. 

다음 단계를 완료하여 첫 암호화 키를 작성하십시오. 

1. {{site.data.keyword.keymanagementserviceshort}} 대시보드에서 **관리** &gt; **키 추가**를 클릭하십시오.
2. 키를 새로 작성하려면 **새 키 생성** 창을 선택하십시오.

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
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 [키의 유형](/docs/services/keymgmt/keyprotect_envelope.html#key_types)입니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 1. 새 키 생성 설정에 대한 설명</caption>
    </table>

3. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 생성**을 클릭하십시오. 

서비스에서 작성된 키는 대칭 256비트 키이며, AES-GCM 알고리즘으로 지원됩니다. 보안 추가를 위해 키가 보안 {{site.data.keyword.cloud_notm}} 데이터 센터에 있는 FIPS 140-2 레벨 2 공인 HSM(Hardware Security Module)에서 생성됩니다. 

## 기존 키 추가
{: #adding_keys}

서비스에 기존 키를 도입하여 BYOK(Bring Your Own Key)의 보안 이점을 사용할 수 있습니다. 

다음 단계를 완료하여 기존 키를 추가하십시오.

1. {{site.data.keyword.keymanagementserviceshort}} 대시보드에서 **관리** &gt; **키 추가**를 클릭하십시오.
2. 기존 키를 업로드하려면 **기존 키 입력** 창을 선택하십시오.

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
        <td>{{site.data.keyword.keymanagementserviceshort}}에서 관리할 [키의 유형](/docs/services/keymgmt/keyprotect_envelope.html#key_types)입니다.</td>
      </tr>
      <tr>
        <td>키 자료</td>
        <td>기존 키를 추가하는 경우에만 필요합니다. 키 자료는 {{site.data.keyword.keymanagementserviceshort}} 서비스에 저장할 데이터 유형입니다(예: 대칭 키). 제공하는 키는 base64로 인코딩되어야 합니다.</td>
      </tr>
      <caption style="caption-side:bottom;">표 2. 기존 키 입력 설정의 설명</caption>
    </table>

3. 키의 세부사항 채우기를 완료한 후 확인하려면 **키 새로 추가**를 클릭하십시오. 

{{site.data.keyword.keymanagementserviceshort}} 대시보드에서 새 키의 일반 특성을 검사할 수 있습니다. 

## 다음에 수행할 작업

이제 키를 사용하여 앱과 서비스를 코딩할 수 있습니다.

- 프로그래밍 방식의 키 관리에 대한 자세한 정보를 찾으려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서에서 코드 샘플 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/apidocs/639){: new_window}을 확인하십시오.
- 데이터를 암호화하고 복호화하기 위해 {{site.data.keyword.keymanagementserviceshort}}에 저장된 키가 작동하는 방식에 대한 예를 보려면 [GitHub에서 샘플 앱 확인![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}을 수행하십시오.
- {{site.data.keyword.keymanagementserviceshort}} 서비스와 다른 클라우드 데이터 솔루션과의 통합에 대해 자세히 알아보려면 [통합 문서를 확인](/docs/services/keymgmt/keyprotect_integration.html)하십시오.
