---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: envelope encryption, key name, create key in different region, delete service instance

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

## How does pricing work for {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} offers a [graduated tier plan](https://{DomainName}/catalog/services/key-protect) with no-charge pricing for users requiring 20 or fewer keys. You can have 20 free keys per {{site.data.keyword.cloud_notm}} account. If your team requires multiple instances of {{site.data.keyword.keymanagementserviceshort}}, {{site.data.keyword.cloud_notm}} adds your active keys across all instances within the account and then applies pricing. 

## What is an active encryption key?
{: #what-is-active-encryption-key}
{: faq}

When you import encryption keys into {{site.data.keyword.keymanagementserviceshort}}, or when you use {{site.data.keyword.keymanagementserviceshort}} to generate keys from its HSMs, those keys become _active_ keys. Pricing is based on all active keys within an {{site.data.keyword.cloud_notm}} account. 

## How should I group and manage my keys?
{: #how-to-group-keys}
{: faq}

From a pricing standpoint, the best way to use {{site.data.keyword.keymanagementserviceshort}} is to create a limited number of root keys, and then use those root keys to encrypt the data encryption keys that are created by an external app or cloud data service. 

To find out more about using root keys to protect data encryption keys, check out [Protecting data with envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## What is a root key?
{: #what-is-root-key}
{: faq}

Root keys are primary resources in {{site.data.keyword.keymanagementserviceshort}}. They are symmetric key-wrapping keys used as roots of trust for protecting other keys that are stored in a data service with [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption). With {{site.data.keyword.keymanagementserviceshort}}, you can create, store, and manage the lifecycle of root keys to achieve full control of other keys stored in the cloud. 

## What is envelope encryption?
{: #what-is-envelope-encryption}
{: faq}

Envelope encryption is the practice of encrypting data with a _data encryption key_, and then encrypting the data encryption key with a highly secure _key-wrapping key_.  Your data is protected at rest by applying multiple layers of encryption. To learn how to enable envelope encryption for your {{site.data.keyword.cloud_notm}} resources, check out [Integrating services](/docs/services/key-protect?topic=key-protect-integrate-services).

## How long can a key name be?
{: #key-names}
{: faq}

You can use a key name that is up to 90 characters in length.

## Can I store personal information as metadata for my keys?
{: #personal-data}
{: faq}

To protect the confidentiality of your personal data, do not store personally identifiable information (PII) as metadata for your keys. Personal information includes your name, address, phone number, email address, or other information that might identify, contact, or locate you, your customers, or anyone else.

You are responsible for ensuring the security of any information that you store as metadata for {{site.data.keyword.keymanagementserviceshort}} resources and encryption keys. For more examples of personal data, see section 2.2 of the [NIST Special Publication 800-122 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
{: important}

## Can keys that are created in one region be used in another region?
{: #keys-across-regions}
{: faq}

Your encryption keys are confined to the region that you created them in. {{site.data.keyword.keymanagementserviceshort}} does not copy or export encryption keys to other regions.

## How do I control who has access to keys?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} supports a centralized access control system, governed by {{site.data.keyword.iamlong}}, to help you manage users and access for your encryption keys. If you are a security admin for your service, you can assign [Cloud IAM roles that correspond to the specific {{site.data.keyword.keymanagementserviceshort}} permissions](/docs/services/key-protect?topic=key-protect-manage-access#roles) you want to grant to members of your team.

## How do I monitor API calls to {{site.data.keyword.keymanagementserviceshort}}?
{: faq}

You can use the {{site.data.keyword.cloudaccesstrailfull_notm}} service to track how users and applications interact with your {{site.data.keyword.keymanagementserviceshort}} service instance. For example, when you create, import, delete, or read a key in {{site.data.keyword.keymanagementserviceshort}}, an {{site.data.keyword.cloudaccesstrailshort}} event is generated. These events are automatically forwarded to the {{site.data.keyword.cloudaccesstrailshort}} service in the same region where the {{site.data.keyword.keymanagementserviceshort}} service is provisioned.

To find out more, check out [Activity Tracker events](/docs/services/key-protect?topic=key-protect-activity-tracker-events).

## What happens when I delete a key?
{: #key-destruction}
{: faq}

When you delete a key, the service marks the key as deleted, and the key transitions to the _Destroyed_ state. Keys in this state are no longer recoverable, and the cloud services that use the key can no longer decrypt data that is associated with the key. Your data remains in those services in its encrypted form. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database. 

Before you delete a key, ensure that you no longer require access to any data that is associated with the key. This action cannot be reversed.

## What happens when I need to deprovision my service instance?
{: #deprovision-service}
{: faq}

If you decide to move on from {{site.data.keyword.keymanagementserviceshort}}, you must delete any remaining keys from your service instance before you can deprovision the service. After you delete your service instance, {{site.data.keyword.keymanagementserviceshort}} uses [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption) to crypto-shred any data that is associated with the service instance. 