---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# {{site.data.keyword.keymanagementserviceshort}} の概説

{{site.data.keyword.keymanagementservicefull}} は、{{site.data.keyword.cloud_notm}} サービス上のアプリの暗号鍵をプロビジョンするときに役立ちます。 このチュートリアルでは、{{site.data.keyword.keymanagementserviceshort}} ダッシュボードを使用して暗号鍵を作成したり、既存の暗号鍵を追加したりする方法を示し、データ暗号化を中央の 1 カ所から管理できるようにします。
{: shortdesc}

## 暗号鍵の概説
{: #get-started-keys}

{{site.data.keyword.keymanagementserviceshort}} ダッシュボードでは、暗号化用の新しい鍵を作成したり、既存の鍵をインポートしたりできます。 

次の 2 つの鍵タイプから選択します。

<dl>
  <dt>ルート鍵</dt>
    <dd>ルート鍵は、ユーザーが {{site.data.keyword.keymanagementserviceshort}} 内で完全に管理する、対称鍵ラップ鍵です。 ルート鍵を使用して、拡張暗号化によって他の暗号鍵を保護できます。詳しくは、<a href="/docs/services/key-protect/concepts/envelope-encryption.html">エンベロープ暗号化</a>を参照してください。</dd>
  <dt>標準鍵</dt>
    <dd>標準鍵は、暗号化に使用される対称鍵です。 標準鍵を使用して、直接、データを暗号化および暗号化解除できます。</dd>
</dl>

## 新しい鍵の作成
{: #create-keys}

[{{site.data.keyword.keymanagementserviceshort}} のインスタンスを作成すると](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps)、サービスで鍵を指定する準備が整います。 

最初の暗号鍵を作成するには、以下の手順を実行します。 

1. {{site.data.keyword.keymanagementserviceshort}} ダッシュボードで、**「管理」** &gt; **「鍵の追加」**をクリックします。
2. 新しい鍵を作成するには、**「新しい鍵の生成 (Generate a new key)」**ウィンドウを選択します。

    鍵の詳細を以下のように指定します。

    <table>
      <tr>
        <th>設定</th>
        <th>説明</th>
      </tr>
      <tr>
        <td>名前</td>
        <td>
          <p>鍵を簡単に識別するための、人間が理解できる固有の別名。</p>
          <p>プライバシーを保護するため、鍵の名前には、個人の名前や場所などの個人情報 (PII) を含めないように注意してください。</p>
        </td>
      </tr>
      <tr>
        <td>鍵のタイプ</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} で管理する<a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">鍵のタイプ</a>。</td>
      </tr>
      <caption style="caption-side:bottom;">表 1. 「新しい鍵の生成 (Generate new key)」の設定の説明</caption>
    </table>

3. 鍵の詳細の記入が完了したら、**「鍵の生成」**をクリックして確認します。 

サービス内で作成される鍵は、AES-GCM アルゴリズムによってサポートされている、対称 256 ビット鍵です。 セキュリティーを強化するために、鍵はセキュアな {{site.data.keyword.cloud_notm}} データ・センターにある FIPS 140-2 レベル 2 認定ハードウェア・セキュリティー・モジュール (HSM) で生成されます。 

## 既存の鍵の追加
{: #add-keys}

既存の鍵をサービスに導入することにより、独自の鍵の持ち込み (BYOK) によるセキュリティー上の利点を活用できます。 

既存の鍵を追加するには、以下の手順を実行します。

1. {{site.data.keyword.keymanagementserviceshort}} ダッシュボードで、**「管理」** &gt; **「鍵の追加」**をクリックします。
2. 既存の鍵をアップロードするには、**「既存の鍵の入力 (Enter existing key)」**ウィンドウを選択します。

    鍵の詳細を以下のように指定します。

    <table>
      <tr>
        <th>設定</th>
        <th>説明</th>
      </tr>
      <tr>
        <td>名前</td>
        <td>
          <p>鍵を簡単に識別するための、人間が理解できる固有の別名。</p>
          <p>プライバシーを保護するため、鍵の名前には、個人の名前や場所などの個人情報 (PII) を含めないように注意してください。</p>
        </td>
      </tr>
      <tr>
        <td>鍵のタイプ</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} で管理する<a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">鍵のタイプ</a>。</td>
      </tr>
      <tr>
        <td>鍵の素材</td>
        <td>{{site.data.keyword.keymanagementserviceshort}} サービスで保管する鍵の素材 (対称鍵など)。提供する鍵は、base64 エンコードでなければなりません。</td>
      </tr>
      <caption style="caption-side:bottom;">表 2. 「既存の鍵の入力 (Enter existing key)」の設定の説明</caption>
    </table>

3. 鍵の詳細の記入が完了したら、**「新しい鍵の追加 (Add new key)」**をクリックして確認します。 

{{site.data.keyword.keymanagementserviceshort}} ダッシュボードで、新しい鍵の一般特性を検査できます。 

## 次に行うこと

これで、鍵を使用してアプリやサービスをコーディングできるようになりました。 ルート鍵をサービスに追加した場合、ルート鍵を使用して、保存データを暗号化する鍵を保護する方法についての詳細を確認することをお勧めします。[鍵のラッピング](/docs/services/key-protect/wrap-keys.html)を確認して開始してください。

- ルート鍵を使用した暗号鍵の管理および保護について詳しくは、[エンベロープ暗号化](/docs/services/key-protect/concepts/envelope-encryption.html)を確認してください。
- {{site.data.keyword.keymanagementserviceshort}} サービスと他のクラウド・データ・ソリューションとの統合について詳しくは、[統合の資料を確認してください](/docs/services/key-protect/integrations/integrate-services.html)。
- プログラムでの鍵の管理について詳しくは、[{{site.data.keyword.keymanagementserviceshort}} API リファレンス資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/apidocs/key-protect){: new_window} を確認してください。
