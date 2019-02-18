---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Security and compliance
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} has data security strategies in place to meet your compliance needs and ensure your data remains secure and protected in the cloud.
{: shortdesc}

## Data security and encryption
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} uses [{{site.data.keyword.cloud_notm}} hardware security modules (HSMs) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/hardware-security-module) to generate provider-managed key material and perform [envelope encryption](/docs/services/key-protect/envelope-encryption.html) operations. HSMs are tamper-resistant hardware devices that store and use cryptographic key material without exposing keys outside of a cryptographic boundary.

Access to the service takes place over HTTPS, and internal {{site.data.keyword.keymanagementserviceshort}} communication uses the Transport Layer Security (TLS) 1.2 protocol to encrypt data in transit.

To learn more about how {{site.data.keyword.keymanagementserviceshort}} meets your compliance requirements, check out [Platform and service compliance](/docs/overview/security.html#compliancetable).

## Data deletion
{: #data-deletion}

When you delete a key, the service marks the key as deleted, and the key transitions to the _Destroyed_ state. Keys in this state are no longer recoverable, and the cloud services that use the key can no longer decrypt data that is associated with the key. Your data remains in those services in its encrypted form. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database. 

Deleting a key in {{site.data.keyword.keymanagementserviceshort}} is a destructive operation. Keep in mind that after you delete a key, the action cannot be reversed, and any data that is associated with the key is immediately lost at the moment the key is deleted. Before you delete a key, review the data that is associated with the key and ensure that you no longer require access to it. Do not delete a key that is actively protecting data in your production environments. 

To help you determine what data is protected by a key, you can view how your {{site.data.keyword.keymanagementserviceshort}} service instance maps to your existing cloud services by running `ibmcloud iam authorization-policies`. To learn more about viewing service authorizations from the console, see [Granting access between services](/docs/iam/authorizations.html#serviceauth).
{: note}
