---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# API 설정
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}}는 암호화 키를 저장하고 검색하고 생성하기 위해 모든 프로그래밍 언어와 함께 사용할 수 있는 REST API를 제공합니다.
{: shortdesc}

## {{site.data.keyword.cloud_notm}} 인증 정보 검색
{: #retrieve-credentials}

API 관련 작업을 수행하려면 서비스 및 인증용 인증 정보를 생성해야 합니다. 

인증 정보를 수집하려면 다음을 수행하십시오

1. [{{site.data.keyword.cloud_notm}} IAM 액세스 토큰을 생성](/docs/services/key-protect?topic=key-protect-retrieve-access-token)하십시오.
2. [{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 고유하게 식별하는 인스턴스 ID를 검색](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID)하십시오.

## API 요청 구성
{: #form-api-request}

서비스에 대한 API 호출을 작성할 때 처음에 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스를 프로비저닝한 방법에 따라 API 요청을 구조화하십시오. 

요청을 빌드하려면 [지역 서비스 엔드포인트](/docs/services/key-protect?topic=key-protect-regions)를 해당 인증용 인증 정보와 쌍으로 묶으십시오. 예를 들어, `us-south` 지역의 서비스 인스턴스를 작성한 경우 다음 엔드포인트 및 API 헤더를 사용하여 서비스에서 키를 찾아보십시오.

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

`<access_token>` 및 `<instance_ID>`를 검색된 서비스와 인증용 인증 정보로 바꾸십시오.

문제가 발생하는 경우 API 요청을 추적하시겠습니까? cURL 요청의 일부로 `-v` 플래그를 포함하면 응답 헤더에 `correlation-id` 값을 갖게 됩니다. 이 값을 사용하여 디버깅을 위해 요청을 상관시키고 추적할 수 있습니다.
{: tip} 

## 다음에 수행할 작업
{: #whats-next-form-api}

Key Protect에서 암호화 키 관리를 시작할 준비가 되었습니다. 프로그래밍 방식의 키 관리에 대해 자세히 알아보려면 [{{site.data.keyword.keymanagementserviceshort}} API 참조 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/key-protect){: new_window}를 확인하십시오.
