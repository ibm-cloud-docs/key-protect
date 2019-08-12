---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect CLI plug-in, CLI reference

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.keymanagementserviceshort}} CLI 参考
{: #cli-reference}

使用 {{site.data.keyword.keymanagementserviceshort}} CLI 插件，可以管理 {{site.data.keyword.keymanagementserviceshort}} 实例中的密钥。
{:shortdesc}

要安装 CLI 插件，请参阅[设置 CLI](/docs/services/key-protect?topic=key-protect-set-up-cli)。 

登录到 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external} 后，您会收到有关更新可用的通知。请确保 CLI 是最新的，以便使用适用于 {{site.data.keyword.keymanagementserviceshort}} CLI 插件的命令和标志。
{: tip}

## ibmcloud kp 命令
{: #ibmcloud-kp-commands}

可以指定以下某个命令：

<table summary="用于管理密钥的命令">
    <thead>
        <th colspan="5">用于管理密钥的命令</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">表 1. 用于管理密钥的命令</caption> 
 </table>

 <table summary="用于管理密钥策略的命令">
    <thead>
        <th colspan="5">用于管理密钥策略的命令</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-policy-list">kp policy list</a></td>
            <td><a href="#kp-policy-get">kp policy get</a></td>
            <td><a href="#kp-policy-set">kp policy set</a></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">表 2. 用于管理密钥策略的命令</caption> 
 </table>

## kp create
{: #kp-create}

在您指定的 {{site.data.keyword.keymanagementserviceshort}} 服务实例中[创建根密钥](/docs/services/key-protect?topic=key-protect-create-root-keys)。 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL]
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

```sh
$ ibmcloud kp create sample-root-key -i $KP_INSTANCE_ID
SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
```
{:screen}

### 必需参数
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>要分配给密钥的人类可读的唯一别名。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>要在服务中存储和管理的 Base64 编码的密钥资料。要导入现有密钥，请提供 256 位密钥。要生成新密钥，请省略 <code>--key-material</code> 参数。</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>只有在创建<a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">标准密钥</a>时，才需要设置此参数。要创建根密钥，请省略 <code>--standard-key</code> 参数。</dd>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp delete
{: #kp-delete}

删除 {{site.data.keyword.keymanagementserviceshort}} 服务中存储的密钥，如[删除密钥](/docs/services/key-protect?topic=key-protect-delete-keys)中所述。

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp delete 584fb0d9-dec2-47b8-bde5-50f05fd66261 -i $KP_INSTANCE_ID
Deleting key: 584fb0d9-dec2-47b8-bde5-50f05fd66261, from instance: 98d39ab8-cf44-4517-9583-2ad05c7e9bd5...

SUCCESS

Deleted Key
584fb0d9-dec2-47b8-bde5-50f05fd66261
```
{: screen}

### 必需参数
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>要删除的密钥的标识。要检索可用密钥列表，请运行 <a href="#kp-list">kp list</a> 命令。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

## kp list
{: #kp-list}

列出 {{site.data.keyword.keymanagementserviceshort}} 服务实例中可用的最后 200 个密钥。

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp list -i $KP_INSTANCE_ID
Retrieving keys...

SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
92e5fab3-00e8-40e9-8a2d-864de334b043   sample-imported-root-key
```
{: screen}

### 必需参数
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp get
{: #kp-get}

检索有关密钥的详细信息，例如，密钥元数据和密钥资料。

如果将密钥指定为根密钥，那么系统无法返回该密钥的密钥资料。

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp get 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Grabbing info for key id: 3df42bc2-a991-41cb-acc2-3f9eab64a63f...

SUCCESS

Key ID                                 Key Name          Description     Creation Date                   Expiration Date
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key   A sample key.   2019-04-02 16:42:47 +0000 UTC   Key does not expire
```
{:screen}

### 必需参数
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>要检索的密钥的标识。要检索可用密钥列表，请运行 <a href="#kp-list">kp list</a> 命令。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp rotate
{: #kp-rotate}

轮换 {{site.data.keyword.keymanagementserviceshort}} 服务中存储的根密钥，如[轮换根密钥](/docs/services/key-protect?topic=key-protect-wrap-keys)中所述。

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL]
```
{: pre}

```sh
$ ibmcloud kp rotate 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Rotating root key...

SUCCESS
```
{:screen}

### 必需参数
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>要轮换的根密钥的标识。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>要用于轮换现有根密钥的 Base64 编码的密钥资料。要轮换最初导入到服务中的密钥，请提供新的 256 位密钥。要轮换最初在 {{site.data.keyword.keymanagementserviceshort}} 中生成的密钥，请省略 <code>--key-material</code> 参数。</dd>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp wrap
{: #kp-wrap}

通过使用您指定的 {{site.data.keyword.keymanagementserviceshort}} 服务实例中存储的根密钥来[打包数据加密密钥](/docs/services/key-protect?topic=key-protect-wrap-keys)。

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### 必需参数
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>要用于打包的根密钥的标识。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>用于进一步保护密钥的附加认证数据 (AAD)。如果在打包时提供了 AAD，那么在解包时也必须提供相同的 AAD。</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>要管理和保护的 Base64 编码的数据加密密钥 (DEK)。要导入现有密钥，请提供 256 位密钥。要生成并打包新的 DEK，请省略 <code>--plaintext</code> 参数。</dd>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

通过使用您指定的 {{site.data.keyword.keymanagementserviceshort}} 服务实例中存储的根密钥来[解包数据加密密钥](/docs/services/key-protect?topic=key-protect-unwrap-keys)。

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### 必需参数
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>用于初始打包请求的根密钥的标识。</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>在初始打包操作期间返回的已加密数据密钥。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>用于进一步保护密钥的附加认证数据 (AAD)。最多可以提供 255 个字符串，以逗号分隔。如果在打包时提供了 AAD，那么在解包时也必须指定相同的 AAD。</p><p><b>重要信息：</b>{{site.data.keyword.keymanagementserviceshort}} 服务不会保存附加认证数据。如果提供 AAD，请将数据保存到安全位置，以确保您可以在随后的解包请求期间访问并提供同一 AAD。</p></dd>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp policy list
{: #kp-policy-list}

列出与指定的根密钥相关联的策略。

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 必需参数
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>要查询的根密钥的标识。要检索可用密钥列表，请运行 <a href="#kp-list">kp list</a> 命令。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #policy-list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp policy get
{: #kp-policy-get}

检索有关密钥策略的详细信息，例如，密钥的自动轮换时间间隔。

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### 必需参数
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>要查询的密钥的标识。要检索可用密钥列表，请运行 <a href="#kp-list">kp list</a> 命令。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #policy-get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

## kp policy set
{: #kp-policy-set}

创建或替换与指定的根密钥相关联的策略。

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE
                 [--policy INTERVAL]
```
{: pre}

### 必需参数
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>要查询的密钥的标识。要检索可用密钥列表，请运行 <a href="#kp-list">kp list</a> 命令。</dd>
   <dt><code>--set-type</code></dt>
        <dd>指定要设置的策略类型。要设置轮换策略，请使用 <code>--set-type rotation</code>。</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>用于标识 {{site.data.keyword.keymanagementserviceshort}} 服务实例的 {{site.data.keyword.cloud_notm}} 实例标识。</dd>
</dl>

### 可选参数
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>指定密钥的轮换时间间隔(以月为单位）。缺省值为 1。</dd>
    <dt><code>-o, --output</code></dt>
        <dd>设置 CLI 输出格式。缺省情况下，会以表格式打印所有命令。要将输出格式更改为 JSON，请使用 <code>--output json</code>。</dd>
</dl>

