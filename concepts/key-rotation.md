---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-14"

keywords: rotate encryption keys, rotate keys automatically, key rotation

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
{:term: .term}

# Rotating encryption keys
{: #key-rotation}

Key rotation takes place when you retire a root key's original key material, and you re-key it by generating new, cryptographic key material.

Rotating keys on a regular basis helps you meet industry standards and cryptographic best practices. The following table describes the main benefits of key rotation:

| Benefit | Description |
| --- | --- |
| Cryptoperiod management for keys | Key rotation limits how long your information is protected by a single key. By rotating your root keys at regular intervals, you also shorten the cryptoperiod of the keys. The longer the lifetime of an encryption key, the higher the probability for a security breach. |
| Incident mitigation | If your organization detects a security issue, you can immediately rotate the key to mitigate or reduce costs that are associated with key compromise. |
{: caption="Table 1. Describes the benefits of key rotation" caption-side="top"}

Key rotation is treated in the NIST Special Publication 800-57, Recommendation for Key Management. To learn more, see [NIST SP 800-57 Pt. 1 Rev. 4.](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}
{: tip}

## Comparing your key rotation options in {{site.data.keyword.keymanagementserviceshort}}
{: #compare-key-rotation-options}

In {{site.data.keyword.keymanagementserviceshort}}, you can [set a rotation policy for a key](/docs/key-protect?topic=key-protect-set-rotation-policy) or [manually rotate the key](/docs/key-protect?topic=key-protect-rotate-keys). 

Rotation options are available only for root keys.
{: note}

<dl>
  <dt>Setting a rotation policy for a key</dt>
    <dd>{{site.data.keyword.keymanagementserviceshort}} helps you simplify rotation for encryption keys by enabling rotation policies for keys that you generate in the service. After you create a root key, you can manage a rotation policy for the key in the or with the {{site.data.keyword.keymanagementserviceshort}} API. <a href="/docs/key-protect?topic=key-protect-key-rotation#rotation-frequency">Choose an automatic rotation interval between 1 - 12 months for your key</a> based on your on-going security needs. When it's time to rotate the key based on the rotation interval that you specify, {{site.data.keyword.keymanagementserviceshort}} automatically replaces the key with new key material.</dd>
  <dt>Rotating keys manually</dt>
    <dd>As a security admin, you might want to have more control over the frequency of rotation for your keys. If you don't want to set an automatic rotation policy for a key, you can manually create a new key to replace an existing key, and then update your applications so that they reference the new key. To simplify this process, you can use {{site.data.keyword.keymanagementserviceshort}} to rotate the key on-demand. In this scenario, {{site.data.keyword.keymanagementserviceshort}} creates and replaces the key on your behalf with each rotation request. The key retains the same metadata and key ID.</dd>
</dl>

## How key rotation works 
{: #how-key-rotation-works}

Key rotation works by securely transitioning key material from an _Active_ to a _Deactivated_ key state. To replace the deactivated or retired key version, the new key version moves into the _Active_ state and becomes available for cryptographic operations.

### Using {{site.data.keyword.keymanagementserviceshort}} to rotate keys
{: #use-key-protect-rotate-keys}

Keep in mind the following considerations as you prepare to use {{site.data.keyword.keymanagementserviceshort}} for rotating root keys.

<dl>
  <dt>Rotating root keys that are generated in {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>You can use {{site.data.keyword.keymanagementserviceshort}} to rotate a root key that was generated in {{site.data.keyword.keymanagementserviceshort}} by setting a rotation policy for the key, or by manually rotating the key. The metadata for the root key, such as its key ID, does not change when you rotate the key.</dd>
  <dt>Rotating root keys that you bring to the service</dt>
    <dd>To rotate a root key that you initially imported to the service, you must generate and provide new key material for the key. You can use {{site.data.keyword.keymanagementserviceshort}} to manually rotate imported root keys by supplying new key material as part of the rotation request. The metadata for the root key, such as its key ID, does not change when you rotate the key. Because you must provide new key material to rotate an imported key, automatic rotation policies are not available for root keys that have imported key material.</dd>
  <dt>Managing retired key versions</dt>
    <dd>{{site.data.keyword.keymanagementserviceshort}} creates a new version of the key with each rotation request. The service retires the old key versions and retains them until the root key is deleted. The retired key versions can no longer be used to wrap keys, but they remain available for unwrap operations. If {{site.data.keyword.keymanagementserviceshort}} detects that you're using a retired key version to unwrap DEKs, the service provides a newly wrapped DEK that's based on the latest key version.</dd>
 <dt>Monitoring key rotation activity in your account</dt>
    <dd>After you rotate a root key, {{site.data.keyword.keymanagementserviceshort}} notifies the {{site.data.keyword.cloud_notm}} data services that use the root key to protect your data. This notification triggers actions in those services to rewrap the key’s associated data encryption keys (DEKs) with the latest key version. <!--After {{site.data.keyword.keymanagementserviceshort}} receives confirmation from those services that all associated DEKs are rewrapped, you receive an event in your Activity Tracker web UI to show that the rotation is complete.-->
    <p class="note"> To enable key rotation options for your {{site.data.keyword.cloud_notm}} data service, the data service must be integrated with {{site.data.keyword.keymanagementserviceshort}}. Refer to the documentation for your {{site.data.keyword.cloud_notm}} data service, or <a href="/docs/key-protect?topic=key-protect-integrate-services">check out our list of integrated services to learn more</a>.</p>
    <p class="tip"> When you rotate a key in {{site.data.keyword.keymanagementserviceshort}}, you're not charged additional fees. You can continue to unwrap your wrapped data encryption keys (WDEKs) with retired key material at no extra cost. For more information about our pricing options, see the <a href="https://{DomainName}/catalog/services/key-protect" target="_blank">{{site.data.keyword.keymanagementserviceshort}} catalog page</a>.</p>
    </dd>
</dl>


{: tip}

### Understanding the key rotation process
{: #understand-key-rotation-process}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API drives the key rotation process.  

The following diagram shows a contextual view of the key rotation functionality.
![The diagram shows a contextual view of key rotation.](../images/key-rotation.svg)

With each rotation request, {{site.data.keyword.keymanagementserviceshort}} creates a new key version by associating new key material with your root key. 

![The diagram shows a micro view the root key stack.](../images/root-key-stack.svg)

To learn how to use the {{site.data.keyword.keymanagementserviceshort}} API to rotate your root keys, see [Rotating keys](/docs/key-protect?topic=key-protect-rotate-keys). 
{: tip}

### Rewrapping data after rotating a key
{: #rewrap-data-after-key-rotation}

After a rotation completes, a new key version becomes available for protecting data encryption keys (DEKs) with [envelope encryption](x9860393){: term}. Retired root key versions moves to the _Deactivated_ state, where they can only be used to unwrap and access older DEKs that aren't yet protected by the latest root key. 

To secure your envelope encryption workflow, [rewrap your DEKs](/docs/key-protect?topic=key-protect-rewrap-keys) after you rotate a root key so that your at-rest data is protected by the newest root key. Alternatively if {{site.data.keyword.keymanagementserviceshort}} detects that you're using retired key versions to unwrap a DEK, the service automatically reencrypts the DEK and returns a wrapped data encryption key (WDEK) that's based on the latest root key. Store and use the new WDEK for future unwrap operations that the DEKs are protected with the newest key version.

To learn how to use the {{site.data.keyword.keymanagementserviceshort}} API to rewrap data encryption keys, see [Rewrapping keys](/docs/key-protect?topic=key-protect-rewrap-keys).
{: tip}

## Frequency of key rotation
{: #rotation-frequency}

After you generate a root key in {{site.data.keyword.keymanagementserviceshort}}, you decide the frequency of its rotation. You might want to rotate your keys due to personnel turnover, process malfunction, or according to your organization's internal key expiration policy. 

Rotate your keys regularly, for example every 30 days, to meet cryptographic best practices. 

| Rotation type | Frequency | Description |
| --- | --- | --- |
| [Policy-based key rotation](/docs/key-protect?topic=key-protect-set-rotation-policy) | Every 1 - 12 months | Choose a rotation interval between 1 - 12 months for your key based on your on-going security needs. After you set an rotation policy for a key, the clock starts immediately based on the initial creation date for the key. For example, if you set a monthly rotation policy for a key that you created on `2019/02/01`, {{site.data.keyword.keymanagementserviceshort}} automatically rotates the key on `2019/03/01`.|
| [Manual key rotation](/docs/key-protect?topic=key-protect-rotate-keys) | Up to one rotation per hour | If you're manually rotating a key, {{site.data.keyword.keymanagementserviceshort}} allows one rotation per hour for each root key. |
{: caption="Table 2. Rotation frequency options for rotating keys in {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

## What's next
{: #rotation-next-steps}

- To learn how to use {{site.data.keyword.keymanagementserviceshort}} to set an automatic rotation policy for an individual key, see [Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy).
- To find out more about manually rotating root keys, see [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).
- To find out more about viewing the key versions that are available for a root key, see [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
