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

# {{site.data.keyword.keymanagementserviceshort}} CLI リファレンス
{: #cli-reference}

{{site.data.keyword.keymanagementserviceshort}} CLI プラグインを使用して、{{site.data.keyword.keymanagementserviceshort}} のインスタンスの鍵を管理できます。
{:shortdesc}

CLI プラグインをインストールするには、[CLI のセットアップ](/docs/services/key-protect?topic=key-protect-set-up-cli)を参照してください。 

[{{site.data.keyword.cloud_notm}} CLI ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli?topic=cloud-cli-overview){: new_window} にログインすると、更新が使用可能な場合は通知されます。 {{site.data.keyword.keymanagementserviceshort}} CLI プラグインで使用可能なコマンドおよびフラグを使用できるように、CLI は必ず最新状態に保ってください。
{: tip}

## ibmcloud kp コマンド
{: #ibmcloud-kp-commands}

以下のいずれかのコマンドを指定できます。

<table summary="鍵を管理するためのコマンド"> 
    <thead>
        <th colspan="5">鍵を管理するためのコマンド</th>
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
    <caption style="caption-side:bottom;">表 1. 鍵を管理するためのコマンド</caption> 
 </table>

## kp create
{: #kp-create}

指定した {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスで[ルート鍵を作成](/docs/services/key-protect?topic=key-protect-create-root-keys)します。 

```sh
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

### 必須パラメーター
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>鍵に割り当てる、人間が理解できる固有の別名。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>サービスで保管および管理する、base64 エンコードの鍵素材。 既存の鍵をインポートする場合は、256 ビットの鍵を指定します。 新しい鍵を生成するには、<code>--key-material</code> パラメーターを省略します。</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>このパラメーターを設定するのは、<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">標準鍵</a>を作成する場合のみです。 ルート鍵を作成するには、<code>--standard-key</code> パラメーターを省略します。</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>

## kp delete
{: #kp-delete}

{{site.data.keyword.keymanagementserviceshort}} サービスで保管されている[鍵を削除](/docs/services/key-protect?topic=key-protect-delete-keys)します。

```sh
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 必須パラメーター
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>削除する鍵の ID。 使用可能な鍵のリストを取得するには、<a href="#kp-list">kp list</a> コマンドを実行します。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

## kp list
{: #kp-list}

{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスで使用可能な最新の 200 個の鍵をリストします。

```sh
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 必須パラメーター
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>

## kp get
{: #kp-get}

鍵メタデータや鍵素材など、鍵に関する詳細を取得します。

ルート鍵として指定された鍵の場合、システムはその鍵の鍵素材を返すことはできません。

```sh
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 必須パラメーター
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>取得する鍵の ID。 使用可能な鍵のリストを取得するには、<a href="#kp-list">kp list</a> コマンドを実行します。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>

## kp rotate
{: #kp-rotate}

{{site.data.keyword.keymanagementserviceshort}} サービスで保管されている[ルート鍵をローテート](/docs/services/key-protect?topic=key-protect-wrap-keys)します。

```sh
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --key-material KEY_MATERIAL] 
```
{: pre}

### 必須パラメーター
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ローテートするルート鍵の ID。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>既存のルート鍵をローテートする際に使用する Base64 エンコードの鍵素材。 サービスに最初にインポートされた鍵をローテートするには、新しい 256 ビットの鍵を指定します。 {{site.data.keyword.keymanagementserviceshort}} で最初に生成された鍵をローテートするには、<code>--key-material</code> パラメーターを省略します。</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>

## kp wrap
{: #kp-wrap}

指定した {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに保管されているルート鍵を使用して、[データ暗号鍵をラップ](/docs/services/key-protect?topic=key-protect-wrap-keys)します。

```sh
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### 必須パラメーター
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ラッピングに使用するルート鍵の ID。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>鍵をさらにセキュアにするために使用される追加認証データ (AAD)。 wrap で指定した場合、unwrap でも指定する必要があります。</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>管理および保護する Base64 エンコードのデータ暗号鍵 (DEK)。 既存の鍵をインポートする場合は、256 ビットの鍵を指定します。 新しい DEK を生成およびラップする場合は、<code>--plaintext</code> パラメーターを省略します。</dd>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスに保管されているルート鍵を使用して、[データ暗号鍵をアンラップ](/docs/services/key-protect?topic=key-protect-unwrap-keys)します。

```sh
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### 必須パラメーター
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>初期ラップ要求に使用したルート鍵の ID。</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>初期ラップ操作で返されたデータ暗号鍵。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>{{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスを識別する {{site.data.keyword.cloud_notm}} インスタンス ID。</dd>
</dl>

### オプション・パラメーター
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>鍵のセキュリティーを高めるために使用された追加認証データ (AAD)。 コンマで区切って最大 255 個のストリングを指定できます。 wrap で AAD を指定した場合、unwrap で同じ AAD を指定する必要があります。</p><p><b>重要:</b> {{site.data.keyword.keymanagementserviceshort}} サービスは、追加認証データを保存しません。 AAD を提供する場合は、同じ AAD にアクセスすることや、後続のアンラップ要求時に同じ AAD を提供することが確実にできるようにするため、データを安全な場所に保存してください。</p></dd>
    <dt><code>--output</code></dt>
        <dd>CLI 出力形式を設定します。すべてのコマンドで、デフォルトの出力形式は表形式です。出力形式を JSON に変更するには <code>--output json</code> を使用します。</dd>
</dl>



