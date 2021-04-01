---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-01"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# FAQs
{: #faqs}

You can use the following FAQs to help you with
{{site.data.keyword.keymanagementservicelong}}.

## How does pricing work for {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} offers a
[graduated tier plan](/catalog/services/key-protect){: external}
with no-charge pricing for users who require 20 or fewer keys. You can have 20
free keys per {{site.data.keyword.cloud_notm}} account. If your team requires
multiple instances of {{site.data.keyword.keymanagementserviceshort}},
{{site.data.keyword.cloud_notm}} adds your active keys across all instances
within the account and then applies pricing.

## What is an active encryption key?
{: #what-is-active-encryption-key}
{: faq}

When you import encryption keys into
{{site.data.keyword.keymanagementserviceshort}}, or when you use
{{site.data.keyword.keymanagementserviceshort}} to generate keys from its HSMs,
those keys become _Active_ keys. Pricing is based on all active keys within an
{{site.data.keyword.cloud_notm}} account.

## How should I group and manage my keys?
{: #how-to-group-keys}
{: faq}
{: support}

From a pricing standpoint, the best way to use
{{site.data.keyword.keymanagementserviceshort}} is to create a limited number of
root keys, and then use those root keys to encrypt the data encryption keys that
are created by an external app or cloud data service.

To find out more about using root keys to protect data encryption keys, check
out
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## What is a root key?
{: #what-is-root-key}
{: faq}
{: support}

Root keys are primary resources in
{{site.data.keyword.keymanagementserviceshort}}. They are symmetric key-wrapping
keys that are used as roots of trust for protecting other keys that are stored
in a data service with
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

With {{site.data.keyword.keymanagementserviceshort}}, you can create, store, and
manage the lifecycle of root keys to achieve full control of other keys stored
in the cloud.

## What is envelope encryption?
{: #what-is-envelope-encryption}
{: faq}
{: support}

Envelope encryption is the practice of encrypting data with a
_data encryption key_, and then encrypting the data encryption key with a highly
secure _key-wrapping key_. Your data is protected at rest by applying multiple
layers of encryption. To learn how to enable envelope encryption for your
{{site.data.keyword.cloud_notm}} resources, check out
[Integrating services](/docs/key-protect?topic=key-protect-integrate-services).

## How long can a key name be?
{: #key-names}
{: faq}

You can use a key name that is up to 90 characters in length.

## Can I store personal information as metadata for my keys?
{: #personal-data}
{: faq}

To protect the confidentiality of your personal data, do not store personally
identifiable information (PII) as metadata for your keys. Personal information
includes your name, address, phone number, email address, or other information
that might identify, contact, or locate you, your customers, or anyone else.

You are responsible for ensuring the security of any information that you store
as metadata for {{site.data.keyword.keymanagementserviceshort}} resources and
encryption keys.

For more examples of personal data, see section 2.2 of the
[NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

## Can encryption keys that are created in one region be used in another region?
{: #keys-across-regions}
{: faq}
{: support}

Your encryption keys can be used to encrypt data stores located anywhere within
IBM Cloud.

## How do I control who has access to keys?
{: #access-control}
{: faq}
{: support}

{{site.data.keyword.keymanagementserviceshort}} supports a centralized access
control system, governed by {{site.data.keyword.iamlong}}, to help you manage
users and access for your encryption keys. If you are a security admin for your
service, you can assign
[Cloud IAM roles that correspond to the specific {{site.data.keyword.keymanagementserviceshort}} permissions](/docs/key-protect?topic=key-protect-manage-access#roles)
you want to grant to members of your team.

## What are differences between the Reader and ReaderPlus roles?
{: #reader-readerplus}
{: faq}

Both the Reader and ReaderPlus roles help you assign read-only access to
{{site.data.keyword.keymanagementserviceshort}} resources.

- As a **Reader**, you can browse a high-level view of keys and perform wrap and
unwrap actions. Readers cannot access or modify key material.
- As a **ReaderPlus**, you can browse a high-level view of keys, access key
material for standard keys, and perform wrap and unwrap actions. The ReaderPlus
role cannot modify key material.

## How do I monitor API calls to {{site.data.keyword.keymanagementserviceshort}}?
{: #monitor-api-calls}
{: faq}
{: support}

You can use the {{site.data.keyword.at_full_notm}} service to track
how users and applications interact with your
{{site.data.keyword.keymanagementserviceshort}} instance. For example,
when you create, import, delete, or read a key in
{{site.data.keyword.keymanagementserviceshort}}, an
{{site.data.keyword.at_short}} event is generated. These events are
automatically forwarded to the {{site.data.keyword.at_short}}
service in the same region where the
{{site.data.keyword.keymanagementserviceshort}} service is provisioned.

To find out more, check out
[{{site.data.keyword.at_full}} events](/docs/key-protect?topic=key-protect-at-events).

## How can I check what data is encrypted by which root key?
{: #check-protected-data}
{: faq}
{: support}

When you use a root key to protect at rest data with envelope encryption, the
cloud services that use the key can create a registration between the key and
the resource that it protects. Registrations are associations between keys and
cloud resources that help you get a full view of which encryption keys protect
what data on IBM Cloud.

You can
[browse the registrations](/docs/key-protect?topic=key-protect-view-protected-resources)
that are available for your keys and cloud resources by using the
{{site.data.keyword.keymanagementserviceshort}} APIs.

## What happens when I delete a key?
{: #key-destruction}
{: faq}
{: support}

When you delete a key, the service marks the key as deleted, and the key
transitions to the _Destroyed_ state. Keys in this state are no longer
recoverable, and the cloud services that use the key can no longer decrypt data
that is associated with the key. Your data remains in those services in its
encrypted form. Metadata that is associated with a key, such as the key's
transition history and name, is kept in the
{{site.data.keyword.keymanagementserviceshort}} database.

Before you delete a key, ensure that you no longer require access to any data
that is associated with the key. This action cannot be reversed.

## What happens if I try to delete a key that's actively encrypting data?
{: #delete-registered-key}
{: faq}
{: support}

For your protection, {{site.data.keyword.keymanagementserviceshort}} prevents
the deletion of a key that's actively encrypting data in the cloud. If you try
to delete a key that's registered with a cloud resource, the action won't
succeed.

If needed, you can
[force deletion on a key](/docs/key-protect?topic=key-protect-delete-keys#delete-key-force)
by using the {{site.data.keyword.keymanagementserviceshort}} APIs.
[Review which resources are encrypted by the key](/docs/key-protect?topic=key-protect-view-protected-resources)
and verify with the owner of the resources to ensure you no longer require
access to that data.

If you can't delete a key because a retention policy exists on the associated
resource, contact the account owner to remove the retention policy on that
resource.
{: note}

## What happens when I disable a key?
{: #key-disable}
{: faq}
{: support}

When you disable a key, the key transitions to the _Suspended_ state. Keys in
this state are no longer available for encrypt or decrypt operations, and any
data that's associated with the key becomes inaccessible.

Disabling a key is a reversible action. You can always
[enable a disabled key](/docs/key-protect?topic=key-protect-disable-keys#enable-api)
and restore access to the data that was previously encrypted with the key.

## What is a dual authorization policy?
{: #dual-auth}
{: faq}

Dual authorization is a two-step process that requires an action from two
approvers to delete a key. By forcing two entities to authorize the deletion of
a key, you minimize the chances of inadvertent deletion or malicious actions.

With {{site.data.keyword.keymanagementserviceshort}}, you can
[enforce dual authorization policies](/docs/key-protect?topic=key-protect-manage-dual-auth)
at the instance level or for individual keys.

## What happens after I enable a dual authorization policy?
{: #enable-dual-auth}
{: faq}

After you enable a dual authorization policy for a
{{site.data.keyword.keymanagementserviceshort}} instance, any keys that you add
to the instance inherit the policy at the key level. Dual authorization policies
for keys cannot be reverted.

If you have existing keys in a {{site.data.keyword.keymanagementserviceshort}}
instance, those keys will continue to require only a single authorization to be
deleted. If you want to enable those keys for dual authorization, you can use
the {{site.data.keyword.keymanagementserviceshort}} APIs to
[set dual authorization policies for those individual keys](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy).

## Can I disable a dual authorization settings for my {{site.data.keyword.keymanagementserviceshort}} instance?
{: #disable-dual-auth}
{: faq}

Yes. If you need to add a key that doesn't require dual authorization to your
{{site.data.keyword.keymanagementserviceshort}} instance, you can always
[disable dual authorization for the {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth#disable-dual-auth-instance-policy-ui)
so that any new or future keys won't require it.

## What happens when I need to delete or deprovision my {{site.data.keyword.keymanagementserviceshort}} instance?
{: #deprovision-service}
{: faq}
{: support}

If you decide to move on from {{site.data.keyword.keymanagementserviceshort}},
you must delete any remaining keys from your
{{site.data.keyword.keymanagementserviceshort}} instance before you can
delete or deprovision the service. After you delete your
{{site.data.keyword.keymanagementserviceshort}} instance,
{{site.data.keyword.keymanagementserviceshort}} uses
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)
to crypto-shred any data that is associated with the
{{site.data.keyword.keymanagementserviceshort}} instance.

## Why does the user interface show unauthorized access?
{: #user-interface-unauthorized}
{: faq}
{: support}

Setting and retrieving the network access policy is only supported through the
application programming interface (API). Network access policy support will be
added to the user interface (UI), command line interface (CLI), and software
development kit (SDK) in the future.

After the network access policy is set to `private-only` the UI cannot be used
for any {{site.data.keyword.keymanagementserviceshort}} actions.

Keys in a `private-only` instance will not be shown in the UI and any
{{site.data.keyword.keymanagementserviceshort}} actions in the UI will return an
unauthorized error (HTTP status code 401).
