---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect CLI plug-in, CLI reference

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.keymanagementserviceshort}} CLI 참조
{: #cli-reference}

{{site.data.keyword.keymanagementserviceshort}} CLI 플러그인을 사용하여 {{site.data.keyword.keymanagementserviceshort}}의 인스턴스에서 키를 관리할 수 있습니다.
{:shortdesc}

CLI 플러그인을 설치하려면 [CLI 설정](/docs/services/key-protect?topic=key-protect-set-up-cli)을 참조하십시오. 

[{{site.data.keyword.cloud_notm}} CLI ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](/docs/cli?topic=cloud-cli-overview){: new_window}에 로그인하는 경우 업데이트가 있으면 알림을 표시합니다. {{site.data.keyword.keymanagementserviceshort}} CLI 플러그인에 사용 가능한 명령 및 플래그를 사용할 수 있도록 CLI를 최신 상태로 유지하십시오.
{: tip}

## ibmcloud kp 명령
{: #ibmcloud-kp-commands}

다음 명령 중 하나를 지정할 수 있습니다.

<table summary="키 관리를 위한 명령"> 
    <thead>
        <th colspan="5">키 관리를 위한 명령</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">표 1. 키 관리를 위한 명령</caption> 
 </table>

## kp create
{: #kp-create}

지정하는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 [루트 키를 작성](/docs/services/key-protect?topic=key-protect-create-root-keys)합니다. 

```sh
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

### 필수 매개변수
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>키에 지정할 사용자가 읽을 수 있는 고유 별명입니다.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>서비스에 저장하고 관리할 base64로 인코딩된 키 자료입니다. 기존 키를 가져오려면 256비트 키를 제공하십시오. 새 키를 생성하려면 <code>--key-material</code> 매개변수를 생략하십시오.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">표준 키</a>를 작성할 경우에만 매개변수를 설정하십시오. 루트 키를 작성하려면 <code>--standard-key</code> 매개변수를 생략하십시오.</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>

## kp delete
{: #kp-delete}

{{site.data.keyword.keymanagementserviceshort}} 서비스에 저장되는 [키를 삭제](/docs/services/key-protect?topic=key-protect-delete-keys)합니다.

```sh
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 필수 매개변수
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>삭제할 키의 ID입니다. 사용 가능한 키의 목록을 검색하려면 <a href="#kp-list">kp list</a> 명령을 실행하십시오.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

## kp list
{: #kp-list}

{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에서 사용 가능한 마지막 200개의 키를 나열합니다.

```sh
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 필수 매개변수
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>

## kp get
{: #kp-get}

키 메타데이터 및 키 자료와 같은 키에 대한 세부사항을 검색합니다.

키를 루트 키로 지정한 경우 시스템은 이 키의 키 자료를 리턴할 수 없습니다.

```sh
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 필수 매개변수
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>검색할 키의 ID입니다. 사용 가능한 키의 목록을 검색하려면 <a href="#kp-list">kp list</a> 명령을 실행하십시오.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>

## kp rotate
{: #kp-rotate}

{{site.data.keyword.keymanagementserviceshort}} 서비스에 저장되는 [루트 키를 순환](/docs/services/key-protect?topic=key-protect-wrap-keys)합니다.

```sh
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --key-material KEY_MATERIAL] 
```
{: pre}

### 필수 매개변수
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>순환할 루트 키의 ID입니다.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>기존 루트 키에 사용할 base64로 인코딩된 키 자료입니다. 초기에 서비스로 가져온 키를 순환하려면 새 256비트 키를 제공하십시오. {{site.data.keyword.keymanagementserviceshort}}에서 초기에 생성된 키를 순환하려면 <code>--key-material</code> 매개변수를 생략하십시오.</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>

## kp wrap
{: #kp-wrap}

지정하는 {{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 저장되는 루트 키를 사용하여 [데이터 암호화 키를 랩핑](/docs/services/key-protect?topic=key-protect-wrap-keys)합니다.

```sh
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### 필수 매개변수
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>랩핑에 사용할 루트 키의 ID입니다.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>키를 보안하는 데 사용되는 추가 인증 데이터(AAD)입니다. 랩핑에 제공된 경우 랩핑 해제에 제공되어야 합니다.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>관리하고 보호할 base64로 인코딩된 데이터 암호화 키(DEK)입니다. 기존 키를 가져오려면 256비트 키를 제공하십시오. 새 DEK를 생성하고 랩핑하려면 <code>--plaintext</code> 매개변수를 생략하십시오.</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스에 저장되는 루트 키를 사용하여 [데이터 암호화 키를 랩핑 해제](/docs/services/key-protect?topic=key-protect-unwrap-keys)합니다.

```sh
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### 필수 매개변수
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>초기 랩핑 요청에 사용한 루트 키의 ID입니다.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>초기 랩핑 오퍼레이션 중에 리턴된 암호화된 데이터 키입니다.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} 서비스 인스턴스를 식별하는 {{site.data.keyword.cloud_notm}} 인스턴스 ID입니다.</dd>
</dl>

### 선택적 매개변수
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>키를 보안하는 데 사용된 추가 인증 데이터(AAD)입니다. 각각 쉼표로 구분하여 최대 255개의 문자열을 제공할 수 있습니다. 랩핑에 AAD를 제공한 경우 랩핑 해제에 동일한 AAD를 지정해야 합니다.</p><p><b>중요사항:</b> {{site.data.keyword.keymanagementserviceshort}} 서비스는 추가 인증 데이터를 저장하지 않습니다. AAD를 제공하는 경우에는 후속 랩핑 해제 요청 중에 동일한 AAD를 액세스 및 제공할 수 있도록 안전한 위치에 데이터를 저장하십시오.</p></dd>
    <dt><code>--output</code></dt>
        <dd>CLI 출력 형식을 설정하십시오. 기본적으로 모든 명령은 표 형식으로 인쇄됩니다. 출력 형식을 JSON으로 변경하려면 <code>--output json</code>을 사용하십시오.</dd>
</dl>



