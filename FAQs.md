---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# FAQs
{: #faqs}

You can use the following FAQs to help you with {{site.data.keyword.keymanagementservicelong}}.

## How long can a key name be?
{: #key-names}
{: faq}

You can use a key name that is up to 90 characters in length.

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

## What happens when I delete a key?
{: #key-destruction}
{: faq}

When you delete a key, the service marks the key as deleted, and the key transitions to the _Destroyed_ state. Keys in this state are no longer recoverable, and the cloud services that use the key can no longer decrypt data that is associated with the key. Your data remains in those services in its encrypted form. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database. 

Before you delete a key, ensure that you no longer require access to any data that is associated with the key. This action cannot be reversed.

## What happens when I need to deprovision my service instance?
{: #deprovision-service}
{: faq}

If you decide to move on from {{site.data.keyword.keymanagementserviceshort}}, you must delete any remaining keys from your service instance before you can deprovision the service. After you delete your service instance, {{site.data.keyword.keymanagementserviceshort}} uses [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption) to crypto-shred any data that is associated with the service instance. 






