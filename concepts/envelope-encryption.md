---

copyright:
  years: 2017, 2020, 2021
lastupdated: "2021-02-09"

keywords: data at rest encryption, envelope encryption, root key, data encryption key, protect data encryption key, encrypt data encryption key, wrap data encryption key, unwrap data encryption key

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

# Protecting data with envelope encryption
{: #envelope-encryption}

Envelope encryption is the practice of encrypting data with a data 
encryption key (DEK) and then encrypting the DEK with a fully 
manageable root key.
{: shortdesc}

When working with sensitive data, it is important to protect the data with a 
data encryption key (DEK). A DEK is an encryption key that is designed to 
encrypt and decrypt data and can be generated and managed by your service or 
an IBM Cloud service. While the DEK secures your data, the DEK itself is also 
susceptible to attacks so you should take preventative measures to protect your 
DEKs with envelope encryption. Envelope encryption is the process of protecting 
your data encryption keys by encrypting the DEK with a [root key](#key-types). 
The same root key is used to decrypt your DEK, which you can use to
decrypt any data encrypted by the DEK.

You can use {{site.data.keyword.keymanagementservicefull}} to create fully managed 
root keys for use with envelope encryption to protect your stored data, which 
offers several benefits:

| Benefit | Description |
| ------- | ----------- |
| Customer-managed encryption keys | With the service, you can provision root keys to protect the security of your encrypted data encryption keys (DEKs) in the cloud. Root keys serve as master key-wrapping keys, which help you manage and safeguard the DEKs provisioned in {{site.data.keyword.cloud_notm}} data services. You decide whether to import your existing root keys, or have {{site.data.keyword.keymanagementserviceshort}} generate them on your behalf. |
| Confidentiality and integrity protection | {{site.data.keyword.keymanagementserviceshort}} uses the Advanced Encryption Standard (AES) algorithm in Galois/Counter Mode (GCM) to protect keys. When you create keys in the service, {{site.data.keyword.keymanagementserviceshort}} generates them within the trust boundary of {{site.data.keyword.cloud_notm}} hardware security modules (HSMs), so only you have access to your encryption keys. |
| Cryptographic shredding of data | If your organization detects a security issue, or your app no longer needs a set of data, you can choose to shred the data permanently from the cloud. When you delete a root key that protects other DEKS, you ensure that the keys' associated data can no longer be accessed or decrypted. |
| Delegated user access control | {{site.data.keyword.keymanagementserviceshort}} supports a centralized access control system to enable granular access for your keys. [By assigning IAM user roles and advanced permissions](/docs/key-protect?topic=key-protect-manage-access#roles), security admins decide who can access which root keys in the service. |
{: caption="Table 1. Describes the benefits of customer-managed encryption" caption-side="top"}

## How it works
{: #overview}

Envelope encryption combines the strength of multiple encryption algorithms to
protect your sensitive data in the cloud. It works by using a root key to 
wrap (encrypt) one or more data encryption keys (DEKs). The root key is fully 
manageable and safeguards your wrapped (encrypted) DEKs with encryption algorithms.

This key wrapping process creates wrapped DEKs that protect them from unauthorized 
access or exposure. Unwrapping a DEK reverses 
the envelope encryption process by using the same root key, resulting in 
decrypted and authenticated DEK. A wrapped DEK cannot be unwrapped without the
associated root key, which adds an extra layer of security.

Envelope encryption is treated briefly in the NIST Special Publication 800-57,
Recommendation for Key Management. To learn more, see
[NIST SP 800-57 Pt. 1 Rev. 4.](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}

## Key types
{: #key-types}

The service supports two key types, root keys and standard keys, for the
advanced encryption and management of data.

<dl>
  <dt>
    Root keys
  </dt>
  <dd>
    <p>
      Root keys are primary resources in
      {{site.data.keyword.keymanagementserviceshort}}. They are symmetric
      key-wrapping keys used as roots of trust for wrapping (encrypting) and
      unwrapping (decrypting) other keys stored in a data service.
    </p>
    <p>
      With {{site.data.keyword.keymanagementserviceshort}}, you can create,
      store, and manage the lifecycle of root keys to achieve full control of
      other DEKs stored in the cloud. Unlike a standard key, the plaintext of 
      a root key can never leave the bounds of the
      {{site.data.keyword.keymanagementserviceshort}} service.
    </p>
  </dd>

  <dt>
    Standard keys
  </dt>
  <dd>
    Standard keys, which can be used as DEKs, can store encrypted data in 
    {{site.data.keyword.keymanagementserviceshort}}. Once your plaintext 
    standard key is generated an protecting underlying data, destroy the 
    plaintext and safely store the encrypted standard key. 
  </dd>
</dl>

## Wrapping keys with envelope encryption
{: #wrapping}

You can wrap one or more DEKs with advanced encryption algorithms by
designating a root key in {{site.data.keyword.keymanagementserviceshort}} that
you can fully manage.

Complete the following steps to encrypt data via envelope encryption:

1. Generate or use a provisioned DEK from an IBM Cloud service to encrypt 
   your sensitive data.
2. [Generate](/docs/key-protect?topic=key-protect-create-root-keys) 
   or [import](/docs/key-protect?topic=key-protect-import-root-keys) 
   a root key that will be used to protect the DEK from step 1.
3. Use the root key to [wrap](/docs/key-protect?topic=key-protect-wrap-keys
   the DEK. To provide maximum encryption security, you can specify additional 
   authenticated data (AAD) during your wrap request. Note that the same 
   authentication data will be required when unwrapping the DEK.
4. Store and maintain the base64 encoded wrapped DEK output in a secure location,
   such as an app or service.

You can generate a new DEK by omitting the `plaintext` property in your 
wrap request. The newly created DEK will automatically be wrapped as part
of the request.
{: note}

## Unwrapping keys
{: #unwrapping}

Unwrapping a data encryption key (DEK) decrypts and authenticates the data
protected within the key.

If your business application needs to access the contents of your wrapped DEKs,
you can use the
[{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect#invoke-an-action-on-a-key){: external}
to send an unwrap request to the service. 


Complete the following steps to encrypt data via envelope encryption:

1. Retrieve the base64 encoded ciphertext of the wrapped DEK.
2. Retrieve the key ID value of the root key.
3. Make an unwrap request to [{{site.data.keyword.keymanagementserviceshort}} Unwrap API](/apidocs/key-protect#invoke-an-action-on-a-key){: external}.
   Note that if you specified additional authenticated data (AAD) in a previous 
   wrap request, you must supply them in your unwrap request.
4. Use the returned base64 encoded plaintext to decrypt the data protected under
   the DEK.

## Integrating with {{site.data.keyword.cloud_notm}} Services
{: #envelope-encryption-integration}

{{site.data.keyword.keymanagementservicefull}} integrates with a number of
{{site.data.keyword.cloud_notm}} services to enable encryption with
customer-managed keys for those services.

Associating a resource in your cloud data service with a root key in
{{site.data.keyword.keymanagementserviceshort}} allows your data to be protected
at rest while having management control of the root key.

For more information on the services that offer integration with
{{site.data.keyword.keymanagementserviceshort}}, see
[Integrating Services](/docs/key-protect?topic=key-protect-integrate-services).
