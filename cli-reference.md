---

copyright:
  years: 2018
lastupdated: "2018-10-04"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# {{site.data.keyword.keymanagementserviceshort}} CLI Reference
{: #key-protect-cli}

You can use {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to manage keys in your instance of {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

To install the CLI plug-in, see [Setting up the CLI](/docs/services/key-protect/set-up-cli.html). 

When you log in to the [{{site.data.keyword.cloud_notm}} CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/index.html#overview){: new_window}, you're notified when updates are available. Be sure to keep your CLI up-to-date so that you can use the available commands and flags.
{: tip}

## ibmcloud kp commands
{: #ibmcloud-kp-commands}

You can specify one of the following commands:

<table summary="Commands for managing keys" 
    <thead>
        <th colspan="5">Commands for managing keys</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Table 1. Commands for managing keys</caption> 
 </table>



## kp create
{: #kp-create}

[Create a root key](/docs/services/key-protect/create-root-keys.html) in the {{site.data.keyword.keymanagementserviceshort}} service instance that you specify. 

```sh
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
```
{:pre}

### Required parameters

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>A unique, human-readable alias to assign to your key.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>The base64 encoded key material that you want to store and manage in the service. To import an existing key, provide a 256-bit key. To generate a new key, omit the <code>--key-material</code> parameter.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Set the parameter only if you want to create a <a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">standard key</a>. To create root key, omit the <code>--standard-key</code> parameter.</dd>
</dl>

## kp delete
{: #kp-delete}

[Delete a key](/docs/services/key-protect/delete-keys.html) that is stored in your {{site.data.keyword.keymanagementserviceshort}} service.

```sh
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Required parameters

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>The ID of the key that you want to delete. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

## kp list
{: #kp-list}

List the last 200 keys that are available in your {{site.data.keyword.keymanagementserviceshort}} service instance.

```sh
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Required parameters

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Wrap a data encryption key](/docs/services/key-protect/wrap-keys.html) by using a root key that is stored in the {{site.data.keyword.keymanagementserviceshort}} service instance that you specify.

```sh
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Required parameters

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you want to use for wrapping.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>The additional authentication data (AAD) that is used to further secure a key. If provided on wrap must be provided on unwrap.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>The base64 encoded data encryption key (DEK) that you want to manage and protect. To import an existing key, provide a 256-bit key. To generate and wrap a new DEK, omit the <code>--plaintext</code> parameter.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Unwrap a data encryption key](/docs/services/key-protect/unwrap-keys.html) by using a root key that is stored in your {{site.data.keyword.keymanagementserviceshort}} service instance.

```sh
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Required parameters

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you used for the initial wrap request.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>The encrypted data key that was returned during the initial wrap operation.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>The additional authentication data (AAD) that was used to further secure a key. You can provide up to 255 strings, each delimited by a comma. If you supplied AAD on wrap, you must specify the same AAD on unwrap.</p><p><b>Important:</b> The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap requests.</p></dd>
</dl>



