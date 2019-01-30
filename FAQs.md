---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-30"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #faqs}

You can use the following FAQs to help you with {{site.data.keyword.keymanagementservicelong}}.

## How long can a key name be?
{: #key-names}
{: faq}

You can use a key name that is up to 90 characters in length.
   
## Can I use language characters as part of the key name?
{: #key-chars}
{: faq}

Language characters, such as Chinese characters, cannot be used as part of the key name.

## What happens when I delete a key?
{: #key-destruction}
{: faq}

When you delete a key, you permanently shred its contents and associated data. The data that was encrypted with the key will no longer be accessible. Encrypted data may still be stored by IBM but cannot be accessed by IBM without the key.

Before you delete a key, ensure that you no longer require access to any data that is associated with the key. 

## Does {{site.data.keyword.keymanagementserviceshort}} retain data after I delete a key?
{: #data-retention}
{: faq}

When you delete a {{site.data.keyword.keymanagementserviceshort}} resource, the key transitions to the [_Destroyed_ state](/docs/services/key-protect/concepts/key-states.html). The service shreds the key material from its primary database and then updates the metadata for the key to indicate that the resource was deleted. To access data that was previously deleted, {{site.data.keyword.keymanagementserviceshort}} must initiate the [{{site.data.keyword.cloud_notm}} disaster recovery](/docs/overview/zero_downtime.html#disaster-recovery) process. All data that is required for disaster recovery is encrypted and cannot be accessed by using normal procedures. 

Remember to assess the type of data that your root key is protecting in the cloud. If needed, use an on-site hardware security module or key management system to back up your root key material securely.
{: tip}

## What happens when I need to deprovision my service instance?
{: #deprovision-service}
{: faq}

If you decide to move on from {{site.data.keyword.keymanagementserviceshort}}, you must delete any remaining keys from your service instance before you can deprovision the service. After you delete your service instance, {{site.data.keyword.keymanagementserviceshort}} uses [envelope encryption](/docs/services/key-protect/envelope-encryption.html) to crypto-shred any data that is associated with the service instance. 

## Can I store personal information as metadata for my keys?
{: #personal-data}
{: faq}

To protect the confidentiality of your personal data, do not store personally identifiable information (PII) as metadata for your keys. Personal information includes your name, address, phone number, email address, or other information that might identify, contact, or locate you, your customers, or anyone else.

You are responsible for ensuring the security of any information that you store as metadata for {{site.data.keyword.keymanagementserviceshort}} resources and encryption keys. For more examples of personal data, see section 2.2 of the [NIST Special Publication 800-122 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
{: important}

## Can keys that are created in one region be used in another region?
{: #keys-across-regions}
{: faq}

Your encryption keys are confined to the region that you created them in. {{site.data.keyword.keymanagementserviceshort}} does not copy or export encryption keys to other regions. 


