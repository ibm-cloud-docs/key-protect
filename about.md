---

copyright:
  years: 2017, 2025
lastupdated: "2025-05-27"

keywords: about Key Protect, about Key Management Service, Key Protect use cases

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

# About {{site.data.keyword.keymanagementserviceshort}}
{: #about}

{{site.data.keyword.keymanagementservicefull}} is a full-service encryption solution that allows data to be secured and stored in {{site.data.keyword.cloud_notm}} using the latest envelope encryption techniques that leverage FIPS 140-2 Level 3 certified cloud-based hardware security modules.
{: shortdesc}

Sensitive data should not be stored on any cloud provider unencrypted (as "plaintext", in other words). But just as with any method of encryption, going back to the earliest known ciphertexts created thousands of years ago, it's important not just to encrypt information so that it cannot be decoded easily but to protect the ciphers used to encrypt and decrypt it (since having a cipher is as good as having the data).

The solution is a key management system like {{site.data.keyword.keymanagementserviceshort}}, which keeps data secure by encrypting the data encryption keys (DEKs) that encrypt your plaintext data with root keys managed by IBM via an impenetrable HSM. In this kind of a system, known as "envelope encryption", the process of decrypting the data means first "unwrapping" the encrypted DEK (opening its envelope, in other words) and then using the DEK to decrypt the data.

For more information about envelope encryption works, check out [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

Unsure which {{site.data.keyword.cloud_notm}} security is right for your use case? Check out [Which data security service is best for me?](/docs/key-protect?topic=key-protect-manage-secrets-ibm-cloud) for more information.
{: tip}

## What {{site.data.keyword.keymanagementserviceshort}} offers
{: #about-offers}

* **Bring your encryption keys to the cloud**: Fully control and strengthen your key management practices by securely exporting symmetric keys from your internal key management infrastructure into {{site.data.keyword.cloud_notm}}.
* **Robust security**: Provision and store keys using FIPS 140-2 Level 3 certified hardware security modules (HSMs). Leverage {{site.data.keyword.cloud_notm}} [Identity and Access Management (IAM) roles](/docs/account?topic=account-userroles) to provide fine-grain access control to your keys. For more information, check out [Understanding your responsibilities with using Key Protect](/docs/key-protect?topic=key-protect-shared-responsibilities).
* **Control and visibility**: Use {{site.data.keyword.logs_full_notm}} to measure how users and applications interact with {{site.data.keyword.keymanagementserviceshort}}. For more information, check out [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs).
* **Simplified billing**: Track subscription and credit spending for all accounts from a single view. To learn more about keys, key versions, and pricing, check out [Pricing](/docs/key-protect?topic=key-protect-pricing-plan).
* **Self-managed encryption**: Create or import root and standard keys protect your data.
* **Flexibility**: Apps on or outside IBM Cloud can integrate with the Key Protect APIs. {{site.data.keyword.keymanagementserviceshort}} integrates easily with a variety of IBM database, storage, container, and ingestion services. For more information, check out [Integrating services](/docs/key-protect?topic=key-protect-integrate-services).
* **Built-in protection**: Deleted keys, and their encrypted data, can never be recovered. Manage your user roles, key states, and set a rotation schedule that works for your use case using the UI, CLI, or API.
* **Application-independent**: Generate, store, retrieve and manage keys independent of application logic.
* **[Key management interoperability protocol (KMIP)](/docs/key-protect?topic=key-protect-kmip)** support, as [certified by VMWare](https://compatibilityguide.broadcom.com/detail?program=kms&productId=60700&persona=live){: external}, can be directly integrated with any service or platform that accepts encryption via a KMIP KMS server. Where other KMS' require third party KMIP server support, support for KMIP is integrated and managed by {{site.data.keyword.keymanagementserviceshort}}. And because KMIP symmetric keys are only [charged as a single key version](/docs/key-protect?topic=key-protect-pricing-plan), you only pay for what you use.

## Reasons to use {{site.data.keyword.keymanagementserviceshort}}
{: #use-cases}

Here are a few common scenarios that explain how {{site.data.keyword.keymanagementserviceshort}} can be used to solve issues faced by businesses operating at scale in production.

| Scenarios | Reasons |
| --------- | ------- |
| You need to create and manage encryption keys that are backed by FIPS 140-2 Level 3 validated hardware. | You can use **{{site.data.keyword.keymanagementserviceshort}} to generate and import encryption keys by using a multi-tenant service with shared hardware. |
| As an IT admin for a large corporation, you need to integrate, track, and rotate encryption keys for many different service offerings. | The {{site.data.keyword.keymanagementserviceshort}} interface simplifies the management of multiple encryption services. With the service, you can manage and sort encryption keys in one centralized location, or you can separate keys by project and house them in different {{site.data.keyword.cloud_notm}} spaces. |
| As a developer, you want to integrate your pre-existing applications, such as self-encrypting storage, to {{site.data.keyword.keymanagementserviceshort}}. | Apps on or outside {{site.data.keyword.cloud_notm}} can integrate with the {{site.data.keyword.keymanagementserviceshort}} APIs. You can use your own existing keys for your apps and import them into {{site.data.keyword.keymanagementserviceshort}}. |
| Your development team has stringent policies, and you need a way to generate and rotate keys. | With {{site.data.keyword.keymanagementserviceshort}}, you can rapidly generate keys from an {{site.data.keyword.cloud_notm}} hardware security module (HSM). When it's time to replace a key, whether it was created using {{site.data.keyword.keymanagementserviceshort}} or imported, you can [rotate the key on-demand](/docs/key-protect?topic=key-protect-rotate-keys) or [set a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy) for the key to meet your on-going security needs. |
| You are a security admin in an industry, such as finance or legal, that must adhere to governance over how data is protected. You need to grant controlled access of keys without compromising the data that it secures. | With the service, you can control user access to manage keys by [assigning different IAM roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles). For example, you can grant read-only access to users who need to view key creation information without viewing the key material. Similarly, users can be assigned the "Manager" role over only a single key, if needed. |
| You want to perform envelope encryption as you move data into the cloud. You need to bring your own master encryption keys, so you can manage and protect other keys that encrypt your data at rest. | With {{site.data.keyword.keymanagementserviceshort}}, you can [wrap (encrypt) your data encryption keys with a highly secure root key](/docs/key-protect?topic=key-protect-envelope-encryption) and also unwrap that key when needed. You can bring your own root keys or create them in the service. |
{: caption="Reasons to use {{site.data.keyword.keymanagementserviceshort}} in various scenarios." caption-side="top"}

{{site.data.keyword.keymanagementserviceshort}} is a cloud-based key management system that provides the best of cost, security, and scale. If you are looking for a dedicated key management solution that supports customer-controlled, cloud-based HSMs [{{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-get-started){: external} integrates with {{site.data.keyword.keymanagementserviceshort}} to enable Keep Your Own Keys (KYOK) for {{site.data.keyword.cloud_notm}}, so your organization has more control and authority over its data. Check out the [{{site.data.keyword.hscrypto}} offering details page](/catalog/services/hyper-protect-crypto-services){: external} to learn more.
{: tip}

## How {{site.data.keyword.keymanagementserviceshort}} works
{: #kp-how}

{{site.data.keyword.keymanagementservicelong_notm}} helps you manage encryption keys throughout your organization by aligning with {{site.data.keyword.cloud_notm}} IAM roles.

An IT or security admin might need advanced permissions to your instance, keys, or key rings that other users, including auditors, might not. For this reason, {{site.data.keyword.keymanagementserviceshort}} maps to established IAM roles to allow fine-grained access for each user as needed. For more information, check out [Managing users and access](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

The following diagram shows how the default IAM roles of manager, reader, and writer can interact with keys that are managed in the service.

![The diagram shows the same components as described in the previous definition list.](images/keys-use-cases.svg){: caption="Shows how different access roles interact with keys." caption-side="bottom"}

While a particular user can be assigned specific roles over specific resources (a user with a "Reader" role at the instance level might be a "Manager" of a particular key or key ring), in general:

* **Readers** can access information about keys.
* **Writers** can use keys with an application or service that is integrated with {{site.data.keyword.keymanagementserviceshort}}
* **Managers** create keys and control their lifecycle (in addition to being able to do everything Readers and Writers can do).

### Architecture overview
{: #about-architecture-overview}

{{site.data.keyword.keymanagementserviceshort}} uses the Advanced Encryption Standard algorithm in Galois/Counter Mode (AES GCM) to wrap and unwrap DEKs. CRKs that are not imported are created with 256-bit key material. Imported CRKs can be have 128, 192, or 256-bit key material.
{: note}

The following architecture diagram shows how {{site.data.keyword.keymanagementserviceshort}} components work to protect your sensitive data and keys.

![The diagram shows how {{site.data.keyword.keymanagementserviceshort}} components protect sensitive data and keys.](images/kp-architecture.svg){: caption="{{site.data.keyword.keymanagementserviceshort}} architecture" caption-side="bottom"}

Access to the {{site.data.keyword.keymanagementserviceshort}} service takes place over HTTPS. All communication uses the Transport Layer Security (TLS) protocol to encrypt data in transit. For more information about TLS and the ciphers supported by {{site.data.keyword.keymanagementserviceshort}}, check out [Data encryption](/docs/key-protect?topic=key-protect-security-and-compliance#data-encryption).
{: note}

| Components | Description |
| ---------- | ----------- |
| {{site.data.keyword.keymanagementserviceshort}} REST API | The {{site.data.keyword.keymanagementserviceshort}} REST API drives encryption key creation and management across {{site.data.keyword.cloud_notm}} services. |
| IBM-managed hardware security module | {{site.data.keyword.cloud_notm}} data centers provide the hardware to protect your keys. Hardware security modules (HSMs) are tamper-resistant hardware devices that store and use cryptographic key material without exposing keys outside of a cryptographic boundary. All cryptographic operations, such as key creation and key rotation, are performed within the HSM. IBM periodically rotates the HSM's master keys, providing an extra layer of security. |
| Customer-managed encryption keys | Root keys are symmetric keys that protect data encryption keys with [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption). Root keys never leave the boundary of the HSM. |
| Dedicated key storage | Key metadata is stored in highly durable, dedicated storage for {{site.data.keyword.keymanagementserviceshort}} that is encrypted at rest with additional application layer encryption. |
| Fine-grained access control | {{site.data.keyword.keymanagementserviceshort}} leverages {{site.data.keyword.cloud_notm}} IAM roles to ensure that users can be assigned appropriate access at the instance, key, and key ring level. |
{: caption="{{site.data.keyword.keymanagementserviceshort}} service components" caption-side="top"}
