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

# 区域和位置
{: #regions-and-locations}

您可以通过指定区域服务端点来连接应用程序和 {{site.data.keyword.keymanagementservicelong_notm}} 服务。
{: shortdesc}

## 可用区域
{: #regions}

{{site.data.keyword.keymanagementserviceshort}} 在以下区域和位置中可用：![该图显示 Key Protect 服务可用的区域。](images/world-map_min.svg)

## 服务端点
{: #endpoints}

如果是以编程方式管理 {{site.data.keyword.keymanagementserviceshort}} 资源，请参阅下表以确定连接到 [{{site.data.keyword.keymanagementserviceshort}} API](https://console.bluemix.net/apidocs/639) 时要使用的 API 端点： 

<table>
    <tr>
        <th>区域名称</th>
        <th>地理位置
</th>
        <th>服务 API 端点</th>
    </tr>
    <tr>
        <td>德国</td>
        <td>德国法兰克福</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>悉尼</td>
        <td>澳大利亚悉尼</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>英国</td>
        <td>英国伦敦</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>美国南部</td>
        <td>美国达拉斯</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">表 1. 显示 {{site.data.keyword.keymanagementserviceshort}} API 的可用端点</caption>
</table>

对于 Cloud Foundry 组织或空间内存在的 {{site.data.keyword.keymanagementserviceshort}} 服务实例，请使用旧的 `https://ibm-key-protect.edge.bluemix.net` 端点来与 {{site.data.keyword.keymanagementserviceshort}} API 进行交互。
{: tip}

有关向 {{site.data.keyword.keymanagementserviceshort}} 进行认证的更多信息，请参阅[访问 API](/docs/services/keymgmt/keyprotect_authentication.html)。
