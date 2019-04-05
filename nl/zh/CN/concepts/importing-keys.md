---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# 将自己的加密密钥引入到云
{: #importing-keys}

加密密钥包含信息子集，例如，帮助您识别密钥的元数据，以及用于加密和解密数据的_密钥资料_。
{: shortdesc}

当您使用 {{site.data.keyword.keymanagementserviceshort}} 创建密钥时，该服务会代表您生成根植于基于云的硬件安全模块 (HSM) 的密钥资料。但是，根据您的业务需求，可能需要根据内部解决方案生成密钥资料，然后再通过将密钥导入到 {{site.data.keyword.keymanagementserviceshort}} 中来将内部部署密钥管理基础架构扩展到云。

<table>
  <th>优点</th>
  <th>描述</th>
  <tr>
    <td>自带密钥 (BYOK)</td>
    <td>您希望通过从内部部署硬件安全模块 (HSM) 生成强密钥来完全控制和加强密钥管理实践。如果选择从内部密钥管理基础架构导出对称密钥，那么可以使用 {{site.data.keyword.keymanagementserviceshort}} 将其安全地引入到云。</td>
  </tr>
  <tr>
    <td>安全导入根密钥资料</td>
    <td>您希望在将密钥导出到云时确保动态密钥资料受到保护。通过<a href="#transport-keys">使用传输密钥</a>将根密钥资料安全地导入您的 {{site.data.keyword.keymanagementserviceshort}} 服务实例，从而减轻中间人攻击。</td>
  </tr>
  <caption style="caption-side:bottom;">表 1. 描述导入密钥资料的优点</caption>
</table>


## 提前计划导入密钥资料
{: #plan-ahead}

在准备好将根密钥资料导入到服务时，请记住以下注意事项。

<dl>
  <dt>查看创建密钥资料的选项</dt>
    <dd>基于您的安全需求，探索创建 256 位对称加密密钥的选项。例如，可以使用内部密钥管理系统（受经过 FIPS 验证的内部部署硬件安全模块 (HSM) 支持）在将密钥引入到云之前生成密钥资料。如果正在构建概念验证，那么还可以使用加密工具包（例如，<a href="https://www.openssl.org/" target="_blank">OpenSSL</a>）来生成可以导入到 {{site.data.keyword.keymanagementserviceshort}} 的密钥资料，以满足您的测试需求。</dd>
  <dt>选择将密钥资料导入到 {{site.data.keyword.keymanagementserviceshort}} 的选项</dt>
    <dd>根据环境或工作负载所需的安全级别，从两个导入根密钥的选项中进行选择。缺省情况下，{{site.data.keyword.keymanagementserviceshort}} 使用传输层安全性 (TLS) 1.2 协议加密正在传输的密钥资料。如果您是首次构建概念验证或试用该服务，那么可以使用此缺省选项将根密钥资料导入到 {{site.data.keyword.keymanagementserviceshort}} 中。如果您的工作负载所需的安全性机制高于 TLS，那么也可以<a href="#transport-keys">使用传输密钥</a>加密根密钥资料并将其导入到服务中。</dd>
  <dt>提前计划加密密钥资料</dt>
    <dd>如果选择使用传输密钥来加密密钥资料，那么请确定在密钥资料上运行 RSA 加密的方法。您必须使用<a href="https://tools.ietf.org/html/rfc3447" target="_blank">针对 RSA 加密的 PKCS #1 V2.1 标准</a>中指定的 <code>RSAES_OAEP_SHA_256</code> 加密方案。查看内部密钥管理系统或内部部署 HSM 的功能来确定您的选项。</dd>
  <dt>管理已导入的密钥资料的生命周期</dt>
    <dd>将密钥资料导入服务之后，请记住，您负责管理密钥的整个生命周期。您可以在决定将密钥上传到服务时，通过使用 {{site.data.keyword.keymanagementserviceshort}} API 设置该密钥的到期日期。但是，如果您希望<a href="/docs/services/key-protect?topic=key-protect-rotate-keys">轮换已导入的根密钥</a>，那么必须生成并提供新的密钥资料，以撤销并替换现有密钥。</dd>
</dl>

## 使用传输密钥
{: #transport-keys}

传输密钥当前为 Beta 功能。Beta 功能可以随时更改，并且未来的更新可能会引入与最新版本不兼容的更改。
{: important}

如果要在将密钥资料导入 {{site.data.keyword.keymanagementserviceshort}} 之前加密密钥资料，可以通过使用 {{site.data.keyword.keymanagementserviceshort}} API 为服务实例创建传输加密密钥。 

传输密钥是 {{site.data.keyword.keymanagementserviceshort}} 中的资源类型，支持将根密钥资料安全导入到服务实例中。通过使用传输密钥在内部部署中加密密钥资料，可以根据指定的策略在将根密钥传输到 {{site.data.keyword.keymanagementserviceshort}} 的过程中提供保护。例如，您可以为传输密钥设置一种根据时间和使用情况计数来限制密钥使用的策略。

### 工作原理
{: #how-transport-keys-work}

{{site.data.keyword.keymanagementserviceshort}} 会在您为服务实例[创建传输密钥](/docs/services/key-protect?topic=key-protect-create-transport-keys)时生成 4096 位的 RSA 密钥。该服务会加密专用密钥，并随后返回可用于加密根密钥资料的公用密钥和可用于导入根密钥资料的导入令牌。 

准备好[导入根密钥](/docs/services/key-protect?topic=key-protect-import-root-keys#api)到 {{site.data.keyword.keymanagementserviceshort}} 时，您需要提供已加密的根密钥资料和用于验证公用密钥完整性的导入令牌。要完成该请求，{{site.data.keyword.keymanagementserviceshort}} 会使用与您的服务实例相关联的专用密钥来解密已加密的根密钥资料。此过程可确保只有在 {{site.data.keyword.keymanagementserviceshort}} 中生成的传输密钥才能解密您导入并存储在服务中的密钥资料。

每个服务实例只能创建一个传输密钥。要了解有关传输密钥的检索限制的更多信息，请[参阅 {{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。
{: note} 

### API 方法
{: #secure-import-api-methods}

{{site.data.keyword.keymanagementserviceshort}} API 在后台驱动传输密钥创建过程。  

下表列出了用于为服务实例设置保险箱并创建传输密钥的 API 方法。

<table>
  <tr>
    <th>方法</th>
    <th>描述</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">创建传输密钥</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">检索传输密钥元数据</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">检索传输密钥和导入令牌</a></td>
  </tr>
  <caption style="caption-side:bottom;">表 2. 描述 {{site.data.keyword.keymanagementserviceshort}} API 方法</caption>
</table>

要了解有关在 {{site.data.keyword.keymanagementserviceshort}} 中以编程方式管理密钥的更多信息，请查看 [{{site.data.keyword.keymanagementserviceshort}} API 参考文档 ![外部链接图标](../../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/key-protect){: new_window}。


## 后续工作

- 要了解如何为服务实例创建传输密钥，请参阅[创建传输密钥](/docs/services/key-protect?topic=key-protect-create-transport-keys)。
- 要了解有关将密钥导入到服务的更多信息，请参阅[导入根密钥](/docs/services/key-protect?topic=key-protect-import-root-keys)。 
