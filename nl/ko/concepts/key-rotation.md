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

# 키 순환
{: #key-rotation}

키 순환은 암호화 키를 폐기하고 새 암호화 키 자료를 생성하여 키를 대체하는 경우 발생합니다. 

정기적으로 키를 순환하면 산업 표준 및 암호화 우수 사례를 충족시키는 데 도움이 됩니다. 다음 표는 키 순환의 기본 이점에 대해 설명합니다. 

<table>
  <th>이점</th>
  <th>설명</th>
  <tr>
    <td>키를 위한 암호 사용 기간 관리</td>
    <td>키 순환은 단일 키로 보호되는 정보의 양을 제한합니다. 정기적으로 루트 키를 순환하여 키의 암호 사용 기간을 줄일 수도 있습니다. 암호화 키의 수명이 길어 질수록 보안 위반의 가능성이 높아집니다. </td>
  </tr>
  <tr>
    <td>인시던트 완화</td>
    <td>조직에서 보안 문제를 발견하는 경우 키 손상과 연관되는 비용을 줄이기 위해 키를 즉시 순환할 수 있습니다.</td>
  </tr>

  <caption style="caption-side:bottom;">표 1. 키 순환의 이점에 대한 설명</caption>
</table>

키 순환은 NIST Special Publication 800-57, Recommendation for Key Management에서 다뤄집니다. 자세히 보려면 [NIST SP 800-57 Pt. 1 Rev. 4. ![외부 링크 아이콘](../../../icons/launch-glyph.svg "외부 링크 아이콘")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}를 참조하십시오.
{: tip}

## 키 순환 작동 방법
{: #how-rotation-works}

암호화 키는 자체 수명 중에 여러 [키 상태](/docs/services/key-protect/concepts/key-states.html)를 거쳐 전이됩니다. _활성_ 상태에서 키는 데이터를 암호화 및 복호화합니다. _비활성화됨_ 상태에서 키는 데이터를 암호화할 수 없으나 복호화에 계속해서 사용할 수 있습니다. _영구 삭제됨_ 상태에서 키는 암호화 또는 복호화에 더 이상 사용될 수 없습니다. 

키 순환은 _활성_에서 _비활성화됨_ 상태로 키 자료를 안전하게 전이하여 작동됩니다. 폐기된 키 자료를 대체하기 위해 새 키 자료가 _활성_ 상태로 이동되고 암호화 오퍼레이션에 사용 가능한 상태가 됩니다. 

{{site.data.keyword.keymanagementserviceshort}}에서 폐기된 루트 키 자료를 추적할 필요 없이 요청 시 루트 키를 순환할 수 있습니다. 다음 다이어그램은 키 순환 기능의 컨텍스트 보기를 표시합니다.
![다이어그램은 키 순환의 컨텍스트 보기를 표시합니다.](../images/key-rotation_min.svg)

순환은 루트 키에만 사용 가능합니다.
{: note}

### 루트 키 순환
{: #rotating-key}

각 순환 요청 시 {{site.data.keyword.keymanagementserviceshort}}는 새 키 자료를 루트 키와 연관시킵니다.  

![다이어그램은 루트 키 스택의 마이크로 보기를 표시합니다.](../images/root-key-stack_min.svg)

순환이 완료되면 새 루트 키 자료는 [엔벨로프 암호화](/docs/services/key-protect/concepts/envelope-encryption.html)로 향후 데이터 암호화 키(DEK)를 보호하는 데 사용할 수 있습니다. 폐기된 키 자료는 _비활성화됨_ 상태로 이동되며, 이 상태는 아직 최신 루트 키 자료로 보호되지 않는 이전 DEK를 랩핑 해제하고 여기에 액세스하는 데에만 사용될 수 있습니다. {{site.data.keyword.keymanagementserviceshort}}에서 사용자가 이전 DEK를 랩핑 해제하기 위해 폐기된 루트 키 자료를 사용 중임을 알게되는 경우, 서비스는 자동으로 DEK를 다시 암호화하고 최신 루트 키 자료에 따라 랩핑된 데이터 암호화 키(WDEK)를 리턴합니다. 향후 랩핑 해제 오퍼레이션에 필요한 새 WDEK를 저장하고 사용하십시오. 그러면 최신 루트 키 자료로 DEK를 보호할 수 있습니다. 

루트 키를 순환하기 위해 {{site.data.keyword.keymanagementserviceshort}} API 사용 방법에 대해 알아보려면 [키 순환](/docs/services/key-protect/rotate-keys.html)을 참조하십시오.

{{site.data.keyword.keymanagementserviceshort}}에서 키를 순환하는 경우 추가 비용이 부과되지 않습니다. 추가 비용 없이 폐기된 키 자료를 사용하여 랩핑된 데이터 암호화 키(WDEK)를 계속해서 랩핑 해제할 수 있습니다. 가격 책정 옵션에 대한 자세한 정보는 [{{site.data.keyword.keymanagementserviceshort}} 카탈로그 페이지](https://{DomainName}/catalog/services/key-protect)를 참조하십시오.
{: tip}

## 키 순환 빈도
{: #rotation-frequency}

{{site.data.keyword.keymanagementserviceshort}}에서 루트 키를 생성한 후 키 순환 빈도를 결정합니다. 이직, 프로세스 결함으로 인해 또는 조직의 내부 키 만기 정책에 따라 키를 순환하려고 할 수 있습니다.  

암호화 우수 사례를 충족시키기 위해 정기적으로(예: 30일마다) 키를 순환하십시오. {{site.data.keyword.keymanagementserviceshort}}는 각 루트 키마다 시간당 한 번의 순환을 허용합니다. 
