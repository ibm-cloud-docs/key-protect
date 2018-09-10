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

# 密钥状态
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} 遵循 [ 密钥状态的 NIST SP 800-57 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window} 所提出的安全准则。
{: shortdesc}

## 密钥状态和转换
{: #key_transitions}

密钥在其生命周期内会经历若干状态转换，这些状态的作用是指示密钥存在了多久以及数据是否受到保护。 

{{site.data.keyword.keymanagementserviceshort}} 提供了一个图形用户界面和一个 REST API，用于跟踪密钥在其生命周期中经历的若干状态移动。下图显示密钥如何经历从其生成到其销毁之间的各个状态。

![该图显示与以下定义表中所述相同的组件。](../images/key-states_min.svg)

<table>
  <tr>
    <th>状态</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>预激活</td>
    <td>密钥最初以<i>预激活</i>状态创建。预激活的密钥不能用于以加密方式保护数据。</td>
  </tr>
  <tr>
    <td>活动</td>
    <td>密钥在激活日期会立即变为<i>活动</i>状态。此转换标志着密钥加密期的开始。没有激活日期的密钥会立即变为活动状态，并且在到期或销毁之前会保持活动状态。</td>
  </tr>
  <tr>
    <td>已停用</td>
    <td>密钥在其到期日期（如果指定）会变为<i>已停用</i>状态。在此状态下，密钥无法以加密方式保护数据，只能变为<i>已销毁</i>状态。</td>
  </tr>
  <tr>
    <td>已销毁</td>
    <td>已删除的密钥处于<i>已销毁</i>状态。处于此状态的密钥不可恢复。与密钥关联的元数据（例如，密钥的转换历史记录和名称）会保存在 {{site.data.keyword.keymanagementserviceshort}} 数据库中。</td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 描述密钥状态和转换。</caption>
</table>

向服务添加密钥后，请使用 {{site.data.keyword.keymanagementserviceshort}} 仪表板或 {{site.data.keyword.keymanagementserviceshort}} REST API 来查看密钥的转换历史记录和配置。出于审计目的，还可以通过将 {{site.data.keyword.keymanagementserviceshort}} 与 {{site.data.keyword.cloudaccesstrailfull}} 相集成来监视密钥的活动跟踪。供应并开始运行这两项服务后，在 {{site.data.keyword.keymanagementserviceshort}} 中创建和删除密钥时，会在 {{site.data.keyword.cloudaccesstrailshort}} 日志中生成并自动收集活动事件。 

有关更多信息，请参阅[监视 {{site.data.keyword.keymanagementserviceshort}} 活动 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](/docs/services/cloud-activity-tracker/services/security_svcs.html#key_protect){: new_window}。
