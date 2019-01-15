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

# 鍵のローテーション
{: #key-rotation}

鍵のローテーションは、暗号鍵を廃止したときに行われ、新しい暗号鍵素材を生成することで、鍵を置き換えます。

定期的に鍵をローテートすると、業界標準や暗号のベスト・プラクティスに準拠するのに役立ちます。以下の表では、鍵のローテーションの主な利点について説明します。

<table>
  <th>利点</th>
  <th>説明</th>
  <tr>
    <td>鍵の暗号期間の管理</td>
    <td>鍵のローテーションにより、単一の鍵で保護される情報量が制限されます。定期的な間隔でルート鍵をローテートすることで、鍵の暗号期間も短くなります。暗号鍵の存続期間が長くなればなるほど、セキュリティー・ブリーチの可能性が高まります。</td>
  </tr>
  <tr>
    <td>インシデントの緩和</td>
    <td>組織でセキュリティー問題が検出された場合、即時に鍵をローテートすることで、鍵漏えいに伴うコストを緩和または削減できます。</td>
  </tr>

  <caption style="caption-side:bottom;">表 1. 鍵のローテーションの利点の説明</caption>
</table>

鍵のローテーションは、NIST Special Publication 800-57 の『Recommendation for Key Management』で説明されています。詳しくは、[NIST SP 800-57 Pt. 1 Rev. 4 ![外部リンク・アイコン](../../../icons/launch-glyph.svg "外部リンク・アイコン")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window} を参照してください。
{: tip}

## 鍵のローテーションの仕組み
{: #how-rotation-works}

暗号鍵は、その存続期間において、さまざまな[鍵の状態](/docs/services/key-protect/concepts/key-states.html)を遷移します。_アクティブ_ 状態では、鍵はデータを暗号化および復号します。_非アクティブ_ 状態では、鍵はデータを暗号化できませんが、復号には引き続き使用可能です。_破棄_ 状態では、鍵は、暗号化にも復号にも使用できなくなっています。

鍵のローテーションは、鍵素材を_アクティブ_ 状態から_非アクティブ_ 状態にセキュアに遷移させることで機能します。廃止された鍵素材を置き換えるために、新しい鍵素材が_アクティブ_ 状態に移行し、暗号操作で使用可能になります。

{{site.data.keyword.keymanagementserviceshort}} では、廃止されたルート鍵素材を追跡することなく、ルート鍵をオンデマンドでローテートできます。以下の図では、鍵のローテーション機能のコンテキスト・ビューを示します。
![この図は、鍵のローテーションのコンテキスト・ビューを示しています。](../images/key-rotation_min.svg)

ローテーションは、ルート鍵に対してのみ使用可能です。
{: note}

### ルート鍵のローテート
{: #rotating-key}

ローテーション要求が行われるごとに、{{site.data.keyword.keymanagementserviceshort}} は新しい鍵素材をルート鍵に関連付けます。 

![この図は、ルート鍵スタックのミクロ・ビューを示しています。](../images/root-key-stack_min.svg)

ローテーションが完了すると、新しいルート鍵素材が、[エンベロープ暗号化](/docs/services/key-protect/concepts/envelope-encryption.html)で今後のデータ暗号鍵 (DEK) を保護するために使用可能になります。廃止された鍵素材は_非アクティブ_ 状態に移行し、最新のルート鍵素材でまだ保護されていない古い DEK のアンラップおよびアクセスにのみ使用できます。廃止されたルート鍵素材を使用して古い DEK をアンラップしていることが {{site.data.keyword.keymanagementserviceshort}} で検出されると、サービスによって自動的に DEK が再暗号化され、最新のルート鍵素材に基づいたラップ済みデータ暗号鍵 (WDEK) が返されます。今後のアンラップ操作用に新しい WDEK が保管されて使用されるため、最新のルート鍵素材で DEK を保護することになります。

{{site.data.keyword.keymanagementserviceshort}} API を使用してルート鍵をローテートする方法については、『[鍵のローテート](/docs/services/key-protect/rotate-keys.html)』を参照してください。

{{site.data.keyword.keymanagementserviceshort}} で鍵をローテートしても、追加料金は発生しません。追加コストなしで、廃止した鍵素材を使用して、ラップ済みデータ暗号鍵 (WDEK) を引き続きアンラップできます。料金オプションについて詳しくは、[{{site.data.keyword.keymanagementserviceshort}} カタログ・ページ](https://{DomainName}/catalog/services/key-protect)を参照してください。
{: tip}

## 鍵のローテーションの頻度
{: #rotation-frequency}

{{site.data.keyword.keymanagementserviceshort}} でルート鍵を生成した後に、そのローテーションの頻度を決定します。従業員の変更やプロセスの誤動作のため、また組織の内部鍵有効期限ポリシーに従って、鍵をローテートできます。 

暗号のベスト・プラクティスに準拠するために、例えば 30 日ごとなど、定期的に鍵をローテートしてください。{{site.data.keyword.keymanagementserviceshort}} では、各ルート鍵を 1 時間に 1 回ローテートできます。
