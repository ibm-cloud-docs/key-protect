---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# 最新情報
{: #releases}

{{site.data.keyword.keymanagementservicefull}} で使用可能な新規フィーチャーに関する最新情報を確認してください。 

## 2019 年 3 月
{: #mar-2019}

### 追加: {{site.data.keyword.keymanagementserviceshort}} でのポリシー・ベースの鍵ローテーションのサポートの追加
{: #added-support-policy-key-rotation}
次の時点の最新情報: 2019 年 3 月 22 日

{{site.data.keyword.keymanagementserviceshort}} を使用して、ルート鍵のローテーション・ポリシーを関連付けることができるようになりました。

詳しくは、[ローテーション・ポリシーの設定](/docs/services/key-protect?topic=key-protect-set-rotation-policy)を参照してください。{{site.data.keyword.keymanagementserviceshort}} における鍵のローテーションのオプションについて詳しくは、[鍵のローテーションのオプションの比較](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options)を参照してください。

### 追加: {{site.data.keyword.keymanagementserviceshort}} でのトランスポート鍵のサポート (ベータ) の追加
次の時点の最新情報: 2019 年 3 月 20 日

{{site.data.keyword.keymanagementserviceshort}} サービス用のトランスポート暗号鍵を作成することによって、クラウドへの暗号鍵のインポートを保護できます。

詳しくは、[クラウドへの独自の暗号鍵の取り込み](/docs/services/key-protect?topic=key-protect-importing-keys)を参照してください。

現在のところ、トランスポート鍵はベータ・フィーチャーです。ベータ・フィーチャーはいつでも変更される可能性があり、将来の更新で最新バージョンと非互換になるような変更が行われる可能性があります。
{: important}

## 2019 年 2 月
{: #feb-2019}

### 変更: レガシー {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス
次の時点の最新情報: 2019 年 2 月 13 日

2017 年 12 月 15 日より前にプロビジョンされた {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスは、Cloud Foundry をベースにしたレガシー・インフラストラクチャーで稼働しています。このレガシー {{site.data.keyword.keymanagementserviceshort}} サービスは、2019 年 5 月 15 日に廃止される予定です。

**廃止による影響**

アクティブな実動鍵が古い {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンス内にある場合、暗号化されたデータにアクセスできなくなる事態を避けるため、必ず 2019 年 5 月 15 日までに新しいサービス・インスタンスに鍵をマイグレーションしてください。[{{site.data.keyword.cloud_notm}} コンソール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/) でリソース・リストにナビゲートすることによって、レガシー・インスタンスを使用しているかどうかを確認できます。ご使用の {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスが {{site.data.keyword.cloud_notm}} リソース・リストの「**Cloud Foundry サービス**」セクションにリストされる場合、または、サービスの操作のターゲット指定に `https://ibm-key-protect.edge.bluemix.net` API エンドポイントを使用している場合、{{site.data.keyword.keymanagementserviceshort}} のレガシー・インスタンスを使用しています。2019 年 5 月 15 日以降、レガシー・エンドポイントにはアクセスできなくなり、サービスを操作のターゲットにできなくなります。

新しいサービス・インスタンスへの暗号鍵のマイグレーションでヘルプが必要な場合、詳しい手順について、[GitHub 内の migration client ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Cloud/kms-migration-client){: new_window} を確認してください。鍵の状況またはマイグレーション・プロセスについて追加の質問がある場合、Terry Mosbaugh ([mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com)) に連絡してください。
{: tip}

## 2018 年 12 月
{: #dec-2018}

### 変更: {{site.data.keyword.keymanagementserviceshort}} API エンドポイント
次の時点の最新情報: 2018 年 12 月 19 日

{{site.data.keyword.cloud_notm}} の新しい統一されたエクスペリエンスに合わせるため、{{site.data.keyword.keymanagementserviceshort}} のサービス API のベース URL が更新されました。

以下の新しい `cloud.ibm.com` エンドポイントを参照するようにアプリケーションを更新することができます。

- `keyprotect.us-south.bluemix.net` は `us-south.kms.cloud.ibm.com` になりました 
- `keyprotect.us-east.bluemix.net` は `us-east.kms.cloud.ibm.com` になりました 
- `keyprotect.eu-gb.bluemix.net` は `eu-gb.kms.cloud.ibm.com` になりました 
- `keyprotect.eu-de.bluemix.net` は `eu-de.kms.cloud.ibm.com` になりました 
- `keyprotect.au-syd.bluemix.net` は `au-syd.kms.cloud.ibm.com` になりました 
- `keyprotect.jp-tok.bluemix.net` は `jp-tok.kms.cloud.ibm.com` になりました 

この時点では、各地域のサービス・エンドポイントとして両方の URL がサポートされています。 

## 2018 年 10 月
{: #oct-2018}

### 追加: {{site.data.keyword.keymanagementserviceshort}} が東京地域に拡大されます
次の時点の最新情報: 2018 年 10 月 31 日

東京地域で {{site.data.keyword.keymanagementserviceshort}} リソースを作成できるようになりました。 

詳しくは、[地域とロケーション](/docs/services/key-protect?topic=key-protect-regions)を参照してください。

### 追加: {{site.data.keyword.keymanagementserviceshort}} CLI プラグイン
次の時点の最新情報: 2018 年 10 月 2 日

{{site.data.keyword.keymanagementserviceshort}} CLI プラグインを使用して {{site.data.keyword.keymanagementserviceshort}} サービス・インスタンスの鍵を管理できるようになりました。

プラグインのインストール方法について詳しくは、[CLI のセットアップ](/docs/services/key-protect?topic=key-protect-set-up-cli)を参照してください。 {{site.data.keyword.keymanagementserviceshort}} CLI について詳しくは、[この CLI のリファレンス資料を確認してください](/docs/services/key-protect?topic=key-protect-cli-reference)。

## 2018 年 9 月
{: #sept-2018}

### 追加: {{site.data.keyword.keymanagementserviceshort}} でのオンデマンドの鍵ローテーションのサポートの追加
次の時点の最新情報: 2018 年 9 月 28 日

{{site.data.keyword.keymanagementserviceshort}} を使用してルート鍵をオンデマンドでローテートできるようになりました。

詳しくは、『[鍵のローテート](/docs/services/key-protect?topic=key-protect-rotate-keys)』を参照してください。

### 追加: クラウド・アプリケーションのエンドツーエンドのセキュリティー・チュートリアル
次の時点の最新情報: 2018 年 9 月 14 日

独自の暗号鍵を使用してストレージ・バケット・コンテンツを暗号化するのに役立つコード・サンプルをお探しですか?

[新しいチュートリアル](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security)に従うことで、クラウド・アプリケーションのエンドツーエンドのセキュリティーを追加する練習ができるようになりました。

詳しくは、[GitHub のサンプル・アプリケーションを確認してください ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}。

### 追加: {{site.data.keyword.keymanagementserviceshort}} がワシントン DC 地域に拡大されます
次の時点の最新情報: 2018 年 9 月 10 日

ワシントン DC 地域で {{site.data.keyword.keymanagementserviceshort}} リソースを作成できるようになりました。 

詳しくは、[地域とロケーション](/docs/services/key-protect?topic=key-protect-regions)を参照してください。

## 2018 年 8 月
{: #aug-2018}

### 変更: {{site.data.keyword.keymanagementserviceshort}} API 資料の URL
次の時点の最新情報: 2018 年 8 月 28 日

{{site.data.keyword.keymanagementserviceshort}} API リファレンスが移動しました。 

以下の場所から API 資料にアクセスできるようになりました。
https://{DomainName}/apidocs/key-protect. 

## 2018 年 3 月
{: #mar-2018}

### 追加: {{site.data.keyword.keymanagementserviceshort}} がフランクフルト地域に拡大されます
次の時点の最新情報: 2018 年 3 月 21 日

フランクフルト地域で {{site.data.keyword.keymanagementserviceshort}} リソースを作成できるようになりました。 

詳しくは、[地域とロケーション](/docs/services/key-protect?topic=key-protect-regions)を参照してください。

## 2018 年 1 月
{: #jan-2018}

### 追加: {{site.data.keyword.keymanagementserviceshort}} がシドニー地域に拡大されます
次の時点の最新情報: 2018 年 1 月 31 日

シドニー地域で {{site.data.keyword.keymanagementserviceshort}} リソースを作成できるようになりました。 

詳しくは、[地域とロケーション](/docs/services/key-protect?topic=key-protect-regions)を参照してください。

## 2017 年 12 月
{: #dec-2017}

### 追加: {{site.data.keyword.keymanagementserviceshort}} で Bring Your Own Key (BYOK) のサポートが追加されます
次の時点の最新情報: 2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} で Bring Your Own Key (BYOK) と顧客管理の暗号化がサポートされるようになりました。

- サービスの主要リソースとして、顧客ルート鍵 (CRK) とも呼ばれる[ルート鍵](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types)が導入されました。 
- {{site.data.keyword.cos_full_notm}} バケットの[エンベロープ暗号化](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how)が有効になりました。

### 追加: {{site.data.keyword.keymanagementserviceshort}} がロンドン地域に拡大されます
次の時点の最新情報: 2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} がロンドン地域で使用可能になりました。 

詳しくは、[地域とロケーション](/docs/services/key-protect?topic=key-protect-regions)を参照してください。

### 変更: {{site.data.keyword.iamshort}} 役割
次の時点の最新情報: 2017 年 12 月 15 日

{{site.data.keyword.keymanagementserviceshort}} リソースで実行できるアクションを決定する {{site.data.keyword.iamshort}} 役割が変更されました。

- `管理者 (Administrator) `は`管理者 (Manager) `になりました
- `エディター`は`ライター`になりました
- `ビューアー`は`リーダー`になりました

詳しくは、[ユーザーのアクセス権限の管理](/docs/services/key-protect?topic=key-protect-manage-access)を参照してください。

## 2017 年 9 月
{: #sept-2017}

### 追加: {{site.data.keyword.keymanagementserviceshort}} で Cloud IAM のサポートが追加されます
次の時点の最新情報: 2017 年 9 月 19 日

{{site.data.keyword.iamshort}} を使用して、{{site.data.keyword.keymanagementserviceshort}} リソースのアクセス・ポリシーを設定して管理できるようになりました。

詳しくは、[ユーザーのアクセス権限の管理](/docs/services/key-protect?topic=key-protect-manage-access)を参照してください。
