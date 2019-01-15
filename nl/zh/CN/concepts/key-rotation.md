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

# 密钥轮换
{: #key-rotation}

当您撤销某个加密密钥，然后通过生成新的密钥资料来替换该密钥时，会发生密钥轮换。

定期轮换密钥有助于符合行业标准和加密最佳实践。下表描述了密钥轮换的主要优点：

<table>
  <th>优点</th>
  <th>描述</th>
  <tr>
    <td>密钥的加密期管理</td>
    <td>密钥轮换可限制受单个密钥保护的信息量。通过定期轮换根密钥，还可以缩短密钥的加密期。加密密钥的生命周期越长，出现安全漏洞的可能性越高。</td>
  </tr>
  <tr>
    <td>事件缓解</td>
    <td>如果您的组织检测到安全问题，您可以立即轮换密钥，以降低或减少与密钥泄漏相关的成本。</td>
  </tr>

  <caption style="caption-side:bottom;">表 1. 描述密钥轮换的优点</caption>
</table>

NIST Special Publication 800-57 Recommendation for Key Management 中对密钥轮换进行了介绍。要了解更多信息，请参阅 [NIST SP 800-57 Pt.1 Rev. 4 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}。
{: tip}

## 密钥轮换的工作原理
{: #how-rotation-works}

密钥在其生命周期内会经历不同的[密钥状态](/docs/services/key-protect/concepts/key-states.html)。状态为_活动_的密钥可以加密和解密数据。状态为_已停用_的密钥无法加密数据，但仍可用于解密。状态为_已销毁_的密钥不能再用于加密或解密。

密钥轮换就是将密钥资料从_活动_状态安全地转换为_已停用_状态。替换已撤销的密钥资料后，新的密钥资料会变为_活动_状态，并且可用于加密操作。

在 {{site.data.keyword.keymanagementserviceshort}} 中，可以按需轮换根密钥，而无需跟踪已撤销的根密钥资料。下图显示了密钥轮换功能的上下文视图。
![该图显示了密钥轮换的上下文视图。](../images/key-rotation_min.svg)

轮换仅适用于根密钥。
{: note}

### 轮换根密钥
{: #rotating-key}

每次收到轮换请求时，{{site.data.keyword.keymanagementserviceshort}} 都会将新的密钥资料与根密钥相关联。 

![该图显示了根密钥堆栈的微观视图。](../images/root-key-stack_min.svg)

轮换完成后，新的根密钥资料即可用于以[包络加密](/docs/services/key-protect/concepts/envelope-encryption.html)方式来保护未来数据加密密钥 (DEK)。已撤销的密钥资料会变为_已停用_状态，并且只能用于解包和访问尚未受最新根密钥资料保护的较旧 DEK。当 {{site.data.keyword.keymanagementserviceshort}} 服务检测到您正在使用已撤销的根密钥资料来解包较旧 DEK 时，它会自动重新加密 DEK，并返回基于最新根密钥资料打包的数据加密密钥 (WDEK)。您可以存储新的 WDEK 并将其用于未来的解包操作中，以便使用最新的根密钥资料来保护 DEK。

要了解如何使用 {{site.data.keyword.keymanagementserviceshort}} API 来轮换根密钥，请参阅[轮换密钥](/docs/services/key-protect/rotate-keys.html)。

在 {{site.data.keyword.keymanagementserviceshort}} 中轮换密钥时，不会收取额外费用。您可以继续使用已撤销的密钥资料来解包已打包的数据加密密钥 (WDEK)，而无需支付额外费用。有关定价选项的更多信息，请参阅 [{{site.data.keyword.keymanagementserviceshort}} 目录页面](https://{DomainName}/catalog/services/key-protect)。
{: tip}

## 密钥轮换的频率
{: #rotation-frequency}

在 {{site.data.keyword.keymanagementserviceshort}} 中生成根密钥后，需要确定其轮换频率。密钥轮换的原因可能是人员流动、流程故障或您组织的内部密钥到期策略。 

根据加密最佳实践，应定期轮换密钥，例如每 30 天。在 {{site.data.keyword.keymanagementserviceshort}} 中，每个根密钥可以每小时轮换一次。
