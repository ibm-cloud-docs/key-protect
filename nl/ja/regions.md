---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect API endpoints, available regions

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
{:deprecated: .deprecated}

# 地域とロケーション
{: #regions}

地域のサービス・エンドポイントを指定することによって、アプリケーションを {{site.data.keyword.keymanagementservicelong}} サービスに接続できます。
{: shortdesc}

## 使用可能な地域
{: #available-regions}

{{site.data.keyword.keymanagementserviceshort}} は、次の地域およびロケーションで使用可能です。
![この図は、Key Protect サービスが使用可能な地域を示しています。](images/world-map_min.svg)

## サービス・エンドポイント
{: #service-endpoints}

{{site.data.keyword.keymanagementserviceshort}} リソースをプログラムで管理している場合は、次の表を参照して、[{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect) への接続時に使用する API エンドポイントを判断してください。 

<table>
    <tr>
        <th>場所</th>
        <th>サービス API エンドポイント</th>
    </tr>
    <tr>
        <td>ダラス</td>
        <td>
            <code>us-south.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>ワシントン DC</td>
        <td>
            <code>us-east.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>ロンドン</td>
        <td>
            <code>eu-gb.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>フランクフルト</td>
        <td>
            <code>eu-de.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>シドニー</td>
        <td>
            <code>au-syd.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>東京</td>
        <td>
            <code>jp-tok.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">表 1. {{site.data.keyword.keymanagementserviceshort}} API の使用可能なエンドポイント</caption>
</table>

引き続き `https://keyprotect.<region>.bluemix.net` を使用して、操作対象のサービスをターゲットにすることができます。あるいは、新しい `cloud.ibm.com` エンドポイントを使用してアプリケーションを更新することもできます。 
{: tip}

{{site.data.keyword.keymanagementserviceshort}} での認証について詳しくは、[API へのアクセス](/docs/services/key-protect?topic=key-protect-set-up-api)を参照してください。
