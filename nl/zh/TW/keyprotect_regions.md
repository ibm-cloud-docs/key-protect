---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 地區及位置
{: #regions-and-locations}

您可以指定地區服務端點，以使用 {{site.data.keyword.keymanagementservicelong_notm}} 服務來連接應用程式。
{: shortdesc}

## 可用的地區
{: #regions}

{{site.data.keyword.keymanagementserviceshort}} 可在下列地區及位置中取得：
![影像顯示可取得 Key Protect 服務的地區。](images/world-map_min.svg)

## 服務端點
{: #endpoints}

如果您要以程式設計方式管理 {{site.data.keyword.keymanagementserviceshort}} 資源，請參閱下表，以決定要在連接至 [{{site.data.keyword.keymanagementserviceshort}} API](https://console.bluemix.net/apidocs/639) 時使用的 API 端點： 

<table>
    <tr>
        <th>地區名稱</th>
        <th>地理位置</th>
        <th>服務 API 端點</th>
    </tr>
    <tr>
        <td>德國</td>
        <td>德國法蘭克福</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>雪梨</td>
        <td>澳洲雪梨</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>英國</td>
        <td>英國倫敦</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>美國南部</td>
        <td>美國達拉斯</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">表 1. 顯示 {{site.data.keyword.keymanagementserviceshort}} API 的可用端點</caption>
</table>

對於存在於 Cloud Foundry 組織或空間內的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，請使用舊式 `https://ibm-key-protect.edge.bluemix.net` 端點以與 {{site.data.keyword.keymanagementserviceshort}} API 互動。
{: tip}

如需向 {{site.data.keyword.keymanagementserviceshort}} 進行鑑別的相關資訊，請參閱[存取 API](/docs/services/keymgmt/keyprotect_authentication.html)。
