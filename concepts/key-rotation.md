---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: rotate encryption keys, rotate keys automatically, key rotation

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

# Rotating encryption keys
{: #key-rotation}

Key rotation takes place when you retire a root key's original key material, and you re-key it by generating new, cryptographic key material.

Rotating keys on a regular basis helps you meet industry standards and cryptographic best practices. The following table describes the main benefits of key rotation:

<table>
  <th>Benefit</th>
  <th>Description</th>
  <tr>
    <td>Cryptoperiod management for keys</td>
    <td>Key rotation limits how long your information is protected by a single key. By rotating your root keys at regular intervals, you also shorten the cryptoperiod of the keys. The longer the lifetime of an encryption key, the higher the probability for a security breach.</td>
  </tr>
  <tr>
    <td>Incident mitigation</td>
    <td>If your organization detects a security issue, you can immediately rotate the key to mitigate or reduce costs that are associated with key compromise.</td>
  </tr>
  <caption style="caption-side:bottom;">Table 1. Describes the benefits of key rotation</caption>
</table>

Key rotation is treated in the NIST Special Publication 800-57, Recommendation for Key Management. To learn more, see [NIST SP 800-57 Pt. 1 Rev. 4. ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}
{: tip}

## Comparing your key rotation options in {{site.data.keyword.keymanagementserviceshort}}
{: #compare-key-rotation-options}

In {{site.data.keyword.keymanagementserviceshort}}, you can [set a rotation policy for a key](/docs/services/key-protect?topic=key-protect-set-rotation-policy) or [rotate the key on-demand](/docs/services/key-protect?topic=key-protect-rotate-keys), without needing to keep track of your retired root key material. 

Rotation options are available only for root keys.
{: note}

<dl>
  <dt>Setting a rotation policy for a key</dt>
    <dd>{{site.data.keyword.keymanagementserviceshort}} helps you simplify rotation for encryption keys by enabling rotation policies for keys that you generate in the service. After you create a root key, you can manage a rotation policy for the key in the {{site.data.keyword.keymanagementserviceshort}} GUI or with the API. <a href="/docs/services/key-protect?topic=key-protect-rotation-frequency">Choose an automatic rotation interval between 1 - 12 months for your key</a> based on your on-going security needs. When it's time to rotate the key based on the rotation interval that you specify, {{site.data.keyword.keymanagementserviceshort}} automatically replaces the key with new key material.</dd>
  <dt>Rotating keys on-demand</dt>
    <dd>As a security admin, you might want to have more control over the frequency of rotation for your keys. If you don't want to set an automatic rotation policy for a key, you can manually create a new key to replace an existing key, and then update your applications so that they reference the new key. To simplify this process, you can use {{site.data.keyword.keymanagementserviceshort}} to rotate the key on-demand. In this scenario, {{site.data.keyword.keymanagementserviceshort}} creates and replaces the key on your behalf with each rotation request. The key retains the same metadata and key ID.</dd>
</dl>

## How key rotation works 
{: #how-key-rotation-works}

Key rotation works by securely transitioning key material from an _Active_ to a _Deactivated_ key state. To replace the deactivated or retired key material, new key material moves into the _Active_ state and becomes available for cryptographic operations.

### Using {{site.data.keyword.keymanagementserviceshort}} to rotate keys
{: #use-key-protect-rotate-keys}

Keep in mind the following considerations as you prepare to use {{site.data.keyword.keymanagementserviceshort}} for rotating root keys.

<dl>
  <dt>Rotating root keys that are generated in {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>You can use {{site.data.keyword.keymanagementserviceshort}} to rotate a root key that was generated in {{site.data.keyword.keymanagementserviceshort}} by setting a rotation policy for the key, or by rotating the key on-demand. The metadata for the root key, such as its key ID, does not change when you rotate the key.</dd>
  <dt>Rotating root keys that you bring to the service</dt>
    <dd>To rotate a root key that you initially imported to the service, you must generate and provide new key material for the key. You can use {{site.data.keyword.keymanagementserviceshort}} to rotate imported root keys on-demand by supplying new key material as part of the rotation request. The metadata for the root key, such as its key ID, does not change when you rotate the key. Because you must provide new key material to rotate an imported key, automatic rotation policies are not available for root keys that have imported key material.</dd>
  <dt>Managing retired key material</dt>
    <dd>{{site.data.keyword.keymanagementserviceshort}} creates new key material after you rotate a root key. The service retires the old key material and retains the retired versions until the root key is deleted. When you use the root key for envelope encryption, {{site.data.keyword.keymanagementserviceshort}} uses only the latest key material that is associated with the key. The retired key material can no longer be used to protect keys, but it remains available for unwrap operations. If {{site.data.keyword.keymanagementserviceshort}} detects that you're using retired key material to unwrap DEKs, the service provides a newly wrapped DEK that's based on the latest root key material. You can use the newly wrapped DEK to re-wrap keys with the latest key material.</dd>
 <dt>Enabling key rotation for {{site.data.keyword.cloud_notm}} data services</dt>
    <dd>To enable these key rotation options for your data service on {{site.data.keyword.cloud_notm}}, the data service must be integrated with {{site.data.keyword.keymanagementserviceshort}}. Refer to the documentation for your {{site.data.keyword.cloud_notm}} data service, or <a href="/docs/services/key-protect?topic=key-protect-integrate-services">check out our list of integrated services to learn more</a>.</dd>
</dl>

When you rotate a key in {{site.data.keyword.keymanagementserviceshort}}, you're not charged additional fees. You can continue to unwrap your wrapped data encryption keys (WDEKs) with retired key material at no extra cost. For more information about our pricing options, see the [{{site.data.keyword.keymanagementserviceshort}} catalog page](https://{DomainName}/catalog/services/key-protect).
{: tip}

### Understanding the key rotation process
{: #understand-key-rotation-process}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API drives the key rotation process.  

The following diagram shows a contextual view of the key rotation functionality.
![The diagram shows a contextual view of key rotation.](../images/key-rotation_min.svg)

With each rotation request, {{site.data.keyword.keymanagementserviceshort}} associates new key material with your root key. 

![The diagram shows a micro view the root key stack.](../images/root-key-stack_min.svg)

After a rotation completes, the new root key material becomes available for protecting future data encryption keys (DEKs) with [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption). Retired key material moves to the _Deactivated_ state, where it can only be used to unwrap and access older DEKs that aren't yet protected by the latest root key material. If {{site.data.keyword.keymanagementserviceshort}} detects that you're using retired root key material to unwrap an older DEK, the service automatically reencrypts the DEK and returns a wrapped data encryption key (WDEK) that's based on the latest root key material. Store and use the new WDEK for future unwrap operations, so you protect your DEKs with the latest root key material.

To learn how to use the {{site.data.keyword.keymanagementserviceshort}} API to rotate your root keys, see [Rotating keys](/docs/services/key-protect?topic=key-protect-rotate-keys).

## Frequency of key rotation
{: #rotation-frequency}

After you generate a root key in {{site.data.keyword.keymanagementserviceshort}}, you decide the frequency of its rotation. You might want to rotate your keys due to personnel turnover, process malfunction, or according to your organization's internal key expiration policy. 

Rotate your keys regularly, for example every 30 days, to meet cryptographic best practices. 

| Rotation type | Frequency | Description
| --- | --- | --- |
| [Policy-based key rotation](/docs/services/key-protect?topic=key-protect-set-rotation-policy) | Every 1 - 12 months | Choose a rotation interval between 1 - 12 months for your key based on your on-going security needs. After you set an rotation policy for a key, the clock starts immediately based on the initial creation date for the key. For example, if you set a monthly rotation policy for a key that you created on `2019/02/01`, {{site.data.keyword.keymanagementserviceshort}} automatically rotates the key on `2019/03/01`.|
| [On-demand key rotation](/docs/services/key-protect?topic=key-protect-rotate-keys) | Up to one rotation per hour | If you're rotating a key on-demand, {{site.data.keyword.keymanagementserviceshort}} allows one rotation per hour for each root key. |
{: caption="Table 2. Rotation frequency options for rotating keys in {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

## What's next
{: #rotation-next-steps}

- To learn how to use {{site.data.keyword.keymanagementserviceshort}} to set an automatic rotation policy for an individual key, see [Setting a rotation policy](/docs/services/key-protect?topic=key-protect-set-rotation-policy).
- To find out more about manually rotating root keys, see [Rotating keys on-demand](/docs/services/key-protect?topic=key-protect-rotate-keys).
