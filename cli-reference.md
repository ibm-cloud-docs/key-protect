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

# {{site.data.keyword.keymanagementserviceshort}} CLI Reference
{: #cli-reference}

You can use {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to manage keys in your instance of {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

To install the CLI plug-in, see [Setting up the CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). 

When you log in to the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}, you're notified when updates are available. Be sure to keep your CLI up-to-date so that you can use the commands and flags that are available for the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in.
{: tip}

## ibmcloud kp commands
{: #ibmcloud-kp-commands}

You can specify one of the following commands:

<table summary="Commands for managing keys"> 
    <thead>
        <th colspan="5">Commands for managing keys</th>
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
    <caption style="caption-side:bottom;">Table 1. Commands for managing keys</caption> 
 </table>

 <table summary="Commands for managing key policies"> 
    <thead>
        <th colspan="5">Commands for managing key policies</th>
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
    <caption style="caption-side:bottom;">Table 2. Commands for managing key policies</caption> 
 </table>

## kp create
{: #kp-create}

[Create a root key](/docs/services/key-protect?topic=key-protect-create-root-keys) in the {{site.data.keyword.keymanagementserviceshort}} service instance that you specify. 

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

### Required parameters
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>A unique, human-readable alias to assign to your key.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>The base64 encoded key material that you want to store and manage in the service. To import an existing key, provide a 256-bit key. To generate a new key, omit the <code>--key-material</code> parameter.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Set the parameter only if you want to create a <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">standard key</a>. To create a root key, omit the <code>--standard-key</code> parameter.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Delete a key](/docs/services/key-protect?topic=key-protect-delete-keys) that is stored in your {{site.data.keyword.keymanagementserviceshort}} service.

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

### Required parameters
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>The ID of the key that you want to delete. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

## kp list
{: #kp-list}

List the last 200 keys that are available in your {{site.data.keyword.keymanagementserviceshort}} service instance.

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

### Required parameters
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Retrieve details about a key, such as the key metadata and key material.

If the key was designated as a root key, the system cannot return the key material for that key.

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

### Required parameters
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>The ID of the key that you want to retrieve. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Rotate a root key](/docs/services/key-protect?topic=key-protect-wrap-keys) that is stored in your {{site.data.keyword.keymanagementserviceshort}} service.

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

### Required parameters
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you want to rotate.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>The base64 encoded key material that you want to use for rotating an existing root key. To rotate a key that was initially imported into the service, provide a new 256-bit key. To rotate a key that was initially generated in {{site.data.keyword.keymanagementserviceshort}}, omit the <code>--key-material</code> parameter.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Wrap a data encryption key](/docs/services/key-protect?topic=key-protect-wrap-keys) by using a root key that is stored in the {{site.data.keyword.keymanagementserviceshort}} service instance that you specify.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Required parameters
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you want to use for wrapping.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>The additional authentication data (AAD) that is used to further secure a key. If provided on wrap must be provided on unwrap.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>The base64 encoded data encryption key (DEK) that you want to manage and protect. To import an existing key, provide a 256-bit key. To generate and wrap a new DEK, omit the <code>--plaintext</code> parameter.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Unwrap a data encryption key](/docs/services/key-protect?topic=key-protect-unwrap-keys) by using a root key that is stored in your {{site.data.keyword.keymanagementserviceshort}} service instance.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Required parameters
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you used for the initial wrap request.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>The encrypted data key that was returned during the initial wrap operation.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>The additional authentication data (AAD) that was used to further secure a key. You can provide up to 255 strings, each delimited by a comma. If you supplied AAD on wrap, you must specify the same AAD on unwrap.</p><p><b>Important:</b> The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap requests.</p></dd>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

List the policies that are associated with the root key that you specify.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Required parameters
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>The ID of the root key that you want to query. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #policy-list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Retrieve details about a key policy, such as the key's automatic rotation interval.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Required parameters
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>The ID of the key that you want to query. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #policy-get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Create or replace the policy that is associated with the root key that you specify.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Required parameters
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>The ID of the key that you want to query. To retrieve a list of your available keys, run the <a href="#kp-list">kp list</a> command.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Specify the type of policy that you want to set. To set a rotation policy, use <code>--set-type rotation</code>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} service instance.</dd>
</dl>

### Optional parameters
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Specify the rotation time interval (in months) for a key. The default value is 1.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use <code>--output json</code>.</dd>
</dl>

