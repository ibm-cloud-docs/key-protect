---

copyright:
  years: 2017, 2022
lastupdated: "2022-04-21"

keywords: FAQ, key protect frequently asked questions, key protect answers

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

You can use the following FAQs to help you with {{site.data.keyword.keymanagementservicelong}}.

## How does pricing work for {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} offers a [graduated tier plan](/catalog/services/key-protect){: external} with no-charge pricing for users who require 20 or fewer keys per {{site.data.keyword.cloud_notm}} account.

Because {{site.data.keyword.keymanagementserviceshort}} pricing is done per key, there is no additional charge to creating additional instances (for example, in different regions). If you have 100 keys associated with your account, you are only charged for 100 keys, regardless of the instances where they were created or imported.



## How is one user's information partitioned from other users' data?
{: #faq-partition-data}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} allows you to have one or more instances which are only accessible to you. Access within these instances (or at the account level), can be controlled by the account owner or a designated admin of that account, allowing the application of the principle of least privilege. One way this is possible is by grouping keys into "key rings", allowing an account owner to assign access to a particular group of keys to a particular group of users. For more information, check out [Grouping keys together using key rings](/docs/key-protect?topic=key-protect-grouping-keys).

## How are instance keys secured?
{: #faq-instance-secured}
{: faq}

Each {{site.data.keyword.keymanagementserviceshort}} instance gets a randomly-generated "instance key-encrypted-key" (IKEK) which is wrapped by the HSM master key, producing a wrapped instance key (WIKEK). No user has access to the WIKEK or the IKEK, and even {{site.data.keyword.IBM_notm}} does not have access to the IKEK. There is no direct or explicit access to the WIKEK by {{site.data.keyword.IBM_notm}}, and it is encrypted by the master key.

## What is an active encryption key?
{: #what-is-active-encryption-key}
{: faq}

When you import encryption keys into {{site.data.keyword.keymanagementserviceshort}}, or when you use {{site.data.keyword.keymanagementserviceshort}} to generate keys from its HSMs, those keys become _Active_ keys. Pricing is based on all active keys within an {{site.data.keyword.cloud_notm}} account.

## How should I group and manage my keys?
{: #how-to-group-keys}
{: faq}
{: support}

You can use {{site.data.keyword.keymanagementservicefull}} to create a group of keys for a target group of users that require the same IAM access permissions by bundling your keys in your {{site.data.keyword.keymanagementserviceshort}} service instance into groups called "key rings". A key ring is a collection of keys, within your service instance, that all require the same IAM access permissions. For example, if you have a group of team members who will need a particular type of access to a specific group of keys, you can create a key ring for those keys and assign the appropriate IAM access policy to the target user group. The users that are assigned access to the key ring can create and manage the resources that exist within the key ring.

To find out more about grouping keys, check out [Grouping keys together using key rings](/docs/key-protect?topic=key-protect-grouping-keys).

## What is a root key?
{: #what-is-root-key}
{: faq}
{: support}

Root keys are primary resources in {{site.data.keyword.keymanagementserviceshort}}. They are symmetric key-wrapping keys that are used as roots of trust for protecting other keys that are stored in a data service with [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

With {{site.data.keyword.keymanagementserviceshort}}, you can create, store, and manage the lifecycle of root keys to achieve full control of other keys stored in the cloud.

After a root key has been created, neither a user nor {{site.data.keyword.IBM_notm}} can see its key material.

## What is a data encryption key (DEK)?
{: #what-is-dek}
{: faq}

A DEK is a key used by services like {{site.data.keyword.cloud_notm}} Object Storage service to perform IBM-managed AES256 encryption of the data stored in the cloud object storage. The DEK keys are randomly generated and stored securely with the cloud object storage service near the resources they encrypt. The DEK is used for default encryption in all cases regardless of whether the customer wants to manage the encryption keys or not. The ICOS DEK is not managed by clients nor do they need to rotate it. For cases where clients do want to manage the encryption, they indirectly control the DEK by wrapping the DEK with their own "root key" stored in their {{site.data.keyword.keymanagementserviceshort}} instance. A root key can be generated or imported, and managed by you in your {{site.data.keyword.keymanagementserviceshort}} instance (for example, by rotating keys).

{{site.data.keyword.keymanagementserviceshort}} can generate DEKs (which wraps keys without passing plaintext) through its HSM.

## What is envelope encryption?
{: #what-is-envelope-encryption}
{: faq}
{: support}

Envelope encryption is the practice of encrypting data with a _data encryption key_, and then encrypting the data encryption key with a highly secure _key-wrapping key_. Your data is protected at rest by applying multiple layers of encryption. To learn more about envelope encryption check out [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## How long can a key name be?
{: #key-names}
{: faq}

You can use a key name that is up to 90 characters in length.

## Can I store personal information as metadata for my keys?
{: #personal-data}
{: faq}

To protect the confidentiality of your personal data, do not store personally identifiable information (PII) as metadata for your keys. Personal information includes your name, address, phone number, email address, or other information that might identify, contact, or locate you, your customers, or anyone else.

You are responsible for ensuring the security of any information that you store as metadata for {{site.data.keyword.keymanagementserviceshort}} resources and encryption keys.

For more examples of personal data, see section 2.2 of the [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

## Can encryption keys that are created in one region be used in another region?
{: #keys-across-regions}
{: faq}
{: support}

Your encryption keys can be used to encrypt data stores located anywhere within {{site.data.keyword.cloud_notm}}.

## How do I control who has access to keys?
{: #access-control}
{: faq}
{: support}

{{site.data.keyword.keymanagementserviceshort}} supports a centralized access control system, governed by {{site.data.keyword.iamlong}}, to help you manage users and access for your encryption keys and allow the principle of least privilege. If you are a security admin for your service, you can assign [{{site.data.keyword.cloud_notm}} IAM roles that correspond to the specific {{site.data.keyword.keymanagementserviceshort}} permissions](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles) you want to grant to members of your team.

One way this is possible is by grouping keys into "key rings", allowing an account owner to assign access to a particular group of keys to a particular group of users. For more information, check out [Grouping keys together using key rings](/docs/key-protect?topic=key-protect-grouping-keys).

## What are differences between the Reader and ReaderPlus roles?
{: #reader-readerplus}
{: faq}

Both the Reader and ReaderPlus roles help you assign read-only access to {{site.data.keyword.keymanagementserviceshort}} resources.

- As a **Reader**, you can browse a high-level view of keys and perform wrap and unwrap actions. Readers cannot access or modify key material.
- As a **ReaderPlus**, you can browse a high-level view of keys, access key material for standard keys, and perform wrap and unwrap actions. The ReaderPlus role cannot modify key material.

## How do I monitor API calls to {{site.data.keyword.keymanagementserviceshort}}?
{: #monitor-api-calls}
{: faq}
{: support}

You can use the {{site.data.keyword.at_full_notm}} service to track how users and applications interact with your {{site.data.keyword.keymanagementserviceshort}} instance. For example, when you create, import, delete, or read a key in {{site.data.keyword.keymanagementserviceshort}}, an {{site.data.keyword.at_short}} event is generated. These events are automatically forwarded to the {{site.data.keyword.at_short}} service in the same region where the {{site.data.keyword.keymanagementserviceshort}} service is provisioned.

To find out more, check out [{{site.data.keyword.at_full}} events](/docs/key-protect?topic=key-protect-at-events).

## How can I check what data is encrypted by which root key?
{: #check-protected-data}
{: faq}
{: support}

When you use a root key to protect at rest data with envelope encryption, the cloud services that use the key can create a registration between the key and the resource that it protects. Registrations are associations between keys and cloud resources that help you get a full view of which encryption keys protect what data on {{site.data.keyword.cloud_notm}}.

You can [browse the registrations](/docs/key-protect?topic=key-protect-view-protected-resources) that are available for your keys and cloud resources by using the {{site.data.keyword.keymanagementserviceshort}} APIs.

## What happens when I delete a key?
{: #key-destruction}
{: faq}
{: support}

In the event that a key is no longer needed or should be removed, {{site.data.keyword.keymanagementserviceshort}} allows you to delete and ultimately purge keys, an action that shreds the key material and makes any of the data encrypted with it inaccessible.

Deleting a key moves it into a _Destroyed_ state, a "soft" deletion in which the key can still be seen and restored for 30 days. After 90 days, the key will be automatically purged, or "hard deleted", and its associated data will be permanently shredded and removed from the {{site.data.keyword.keymanagementserviceshort}} service. If it is desirable that a key be purged sooner than 90 days, it is also possible to hard delete a key four hours after it has been moved into the _Destroyed_ state.

After a key has been deleted, any data that is encrypted by the key becomes inaccessible, though this can be reversed if the key is restored within the 30-day time frame. After 30 days, key metadata, registrations, and policies are available for up to 90 days, at which point the key becomes eligible to be purged. Note that once a key is no longer restorable and has been purged, its associated data can no longer be accessed. As a result, [destroying resources](/docs/key-protect?topic=key-protect-security-and-compliance#data-deletion) is not recommended for production environments unless absolutely necessary.

## What happens if I try to delete a key that's actively encrypting data?
{: #delete-registered-key}
{: faq}
{: support}

For your protection, {{site.data.keyword.keymanagementserviceshort}} prevents the deletion of a key that's actively encrypting data in the cloud. If you try to delete a key that's registered with a cloud resource, the action won't succeed.

If needed, you can [force deletion on a key](/docs/key-protect?topic=key-protect-delete-keys#delete-keys-force-delete) by using the {{site.data.keyword.keymanagementserviceshort}} APIs. [Review which resources are encrypted by the key](/docs/key-protect?topic=key-protect-view-protected-resources)and verify with the owner of the resources to ensure you no longer require access to that data.

If you can't delete a key because a retention policy exists on the associated resource, contact the account owner to remove the retention policy on that resource.
{: note}

## What happens when I disable a key?
{: #key-disable}
{: faq}
{: support}

When you disable a key, the key transitions to the _Suspended_ state. Keys in this state are no longer available for encrypt or decrypt operations, and any data that's associated with the key becomes inaccessible.

Disabling a key is a reversible action. You can always [enable a disabled key](/docs/key-protect?topic=key-protect-disable-keys#enable-api) and restore access to the data that was previously encrypted with the key.

## What is a dual authorization policy?
{: #dual-auth}
{: faq}

Dual authorization is a two-step process that requires an action from two approvers to delete a key. By forcing two entities to authorize the deletion of a key, you minimize the chances of inadvertent deletion or malicious actions.

With {{site.data.keyword.keymanagementserviceshort}}, you can [enforce dual authorization policies](/docs/key-protect?topic=key-protect-manage-dual-auth) at the instance level or for individual keys.

## What happens after I enable a dual authorization policy?
{: #enable-dual-auth}
{: faq}

After you enable a dual authorization policy for a {{site.data.keyword.keymanagementserviceshort}} instance, any keys that you add to the instance inherit the policy at the key level. Dual authorization policies for keys cannot be reverted.

If you have existing keys in a {{site.data.keyword.keymanagementserviceshort}} instance, those keys will continue to require only a single authorization to be deleted. If you want to enable those keys for dual authorization, you can use the {{site.data.keyword.keymanagementserviceshort}} APIs t [set dual authorization policies for those individual keys](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy).

## Can I disable a dual authorization settings for my {{site.data.keyword.keymanagementserviceshort}} instance?
{: #disable-dual-auth}
{: faq}

Yes. If you need to add a key that doesn't require dual authorization to your {{site.data.keyword.keymanagementserviceshort}} instance, you can always [disable dual authorization for the {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth#disable dual-auth-instance-policy-ui) so that any new or future keys won't require it.

## What happens when I need to delete or deprovision my {{site.data.keyword.keymanagementserviceshort}} instance?
{: #deprovision-service}
{: faq}
{: support}

If you decide to move on from {{site.data.keyword.keymanagementserviceshort}}, you must delete any remaining keys from your {{site.data.keyword.keymanagementserviceshort}} instance before you can delete or deprovision the service. After you delete your {{site.data.keyword.keymanagementserviceshort}} instance, {{site.data.keyword.keymanagementserviceshort}} uses[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption) to crypto-shred any data that is associated with the {{site.data.keyword.keymanagementserviceshort}} instance.

## Why does the user interface show unauthorized access?
{: #user-interface-unauthorized}
{: faq}
{: support}

Setting and retrieving the network access policy is only supported through the application programming interface (API). Network access policy support will be added to the user interface (UI), command line interface (CLI), and software development kit (SDK) in the future.

After the network access policy is set to `private-only` the UI cannot be used for any {{site.data.keyword.keymanagementserviceshort}} actions.

Keys in a `private-only` instance will not be shown in the UI and any {{site.data.keyword.keymanagementserviceshort}} actions in the UI will return an unauthorized error (HTTP status code 401).


