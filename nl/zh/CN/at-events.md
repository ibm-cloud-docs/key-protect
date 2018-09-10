---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.cloudaccesstrailshort}} 事件
{: #at-events}

使用 {{site.data.keyword.cloudaccesstrailfull}} 服务以跟踪用户和应用程序如何与 {{site.data.keyword.keymanagementservicefull}} 交互。
{: shortdesc}

{{site.data.keyword.cloudaccesstrailfull_notm}} 服务记录由用户启动的会更改 {{site.data.keyword.cloud_notm}} 中服务状态的活动。 

有关更多信息，请参阅 [{{site.data.keyword.cloudaccesstrailshort}} 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla){: new_window}。

## 事件列表
{: #events}

下表列出生成事件的操作：

<table>
    <tr>
        <th>操作</th>
        <th>描述</th>
    </tr>
    <tr>
        <td>kms.secrets.create</td>
        <td>创建密钥</td>
    </tr>
    <tr>
        <td>kms.secrets.read</td>
        <td>按标识检索密钥</td>
    </tr>
   <tr>
        <td>kms.secrets.delete</td>
        <td>按标识删除密钥</td>
    </tr>
    <tr>
        <td>kms.secrets.list</td>
        <td>检索密钥列表</td>
    </tr>
    <tr>
        <td>kms.secrets.head</td>
        <td>检索密钥数量</td>
    </tr>
     <tr>
        <td>kms.secrets.wrap</td>
        <td>打包密钥</td>
    </tr>
     <tr>
        <td>kms.secrets.unwrap</td>
        <td>解包密钥</td>
    </tr>
    <caption style="caption-side:bottom;">表 1. 生成 {{site.data.keyword.cloudaccesstrailfull_notm}} 事件的操作</caption>
</table>

## 查看事件的位置
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} 事件在 {{site.data.keyword.cloudaccesstrailshort}} **帐户域**中提供，该域在生成这些事件的 {{site.data.keyword.cloud_notm}} 区域中提供。

例如，在 {{site.data.keyword.keymanagementserviceshort}} 中创建、导入、删除或读取密钥时，将生成 {{site.data.keyword.cloudaccesstrailshort}} 事件。这些事件将自动转发至在供应 {{site.data.keyword.keymanagementserviceshort}} 服务的区域中的 {{site.data.keyword.cloudaccesstrailshort}} 服务。

要监视 API 活动，必须在供应 {{site.data.keyword.keymanagementserviceshort}} 服务的区域中的可用空间中供应 {{site.data.keyword.cloudaccesstrailshort}} 服务。然后，如果您具有轻量套餐，那么可通过 {{site.data.keyword.cloudaccesstrailshort}} UI 中的帐户视图查看事件，如果是高端套餐，那么通过 Kibana 查看事件。

## 其他信息
{: #info}

由于加密密钥的信息敏感度，当作为针对 {{site.data.keyword.keymanagementserviceshort}} 服务的 API 调用的结果生成事件时，生成的事件不包含有关密钥的详细信息。事件包含可用于在云环境中内部标识密钥的相关标识。相关标识是作为 `responseHeader.content` 字段的一部分返回的字段。您可以使用此信息以将 {{site.data.keyword.keymanagementserviceshort}} 密钥与通过 {{site.data.keyword.cloudaccesstrailshort}} 事件报告的操作的信息相关联。
