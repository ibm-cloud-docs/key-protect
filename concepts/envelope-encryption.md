---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-07"

keywords: envelope encryption, wrap data encryption key, unwrap data encryption key

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

{{site.data.keyword.keymanagementserviceshort}} uses envelope encryption
to assist in protecting your {{site.data.keyword.keymanagementserviceshort}}
data. Envelope encryption involves encrypting your data with a Data Encryption Key,
then encrypting the Data Encryption Key with a root key. This topic describes
the process of envelope encryption and how to use {{site.data.keyword.keymanagementserviceshort}}
to encrypt and decrypt your data.
{: shortdesc}

When working with sensitive data, it is important to use advanced encryption
techniques to prevent a data breach. If you have large amounts of confidential data,
it is often helpful to use a Key Management System to assist in keeping your data secure.
{{site.data.keyword.keymanagementserviceshort}} uses the envelope encryption technique
to keep your data resilient. Envelope encryption is the process of using encrypted keys,
Data Encryption Keys and Root Keys, to protect your sensitive data.

Imagine that you plan to send a letter to a colleague. You want to discuss information that is
highly sensitive, so you generate a secret code (Data Encryption Key) that is used to write
(encrypt) the message in the letter. The letter is delivered to a mailbox (wrapped Data Encryption Key)
that can only be opened by those with a copy of the mailbox key (Root key), including the colleague. Anyone
who does not have an exact copy of the key will be unable to open the mailbox and see it's contents. When
your colleague uses the key to unlock (unencrypt) the mailbox, they will need to know the secret code
that the letter is written in to be able to understand the message. Everyone who is not aware of the secret
code will conclude that the letter is a random mix of characters and will not be able to understand the letter's
contents.

Data encryption keys (DEKs) are designed to encrypt your data and can be generated and
managed by your service or an IBM Cloud service.
{: note}


Envelope encryption offers several benefits for protecting your data:
- Protection under a combination of multiple algorithms
    Envelope encryption uses the best benefits from symmetric and public key algorithms to keep your keys secure.
    1. Symmetric key algorithms work faster, are more scalable, and more secure than public key algorithms. Public key algorithms
        use complicated mathematics that increase computational overhead, especially when dealing with large volumes
        of data. Public key algorithms are also more susceptible to brute force attacks due to having a private key algorithm component that is
        easily recognizable by hackers. Symmetric key algorithms requires less computed power and are resistant to
        brute force attacks due to having a less recognizable structure.
    2. Public key algorithms allow for easier access control when granting access to keys at an individual level
        compared to symmetric key algorithms. Symmetric key algorithms have a key exchange problem, which is that
        access to a secret key can only be exchanged through a secure transfer. By using public key algorithms,
        encrypted DEKs (wDEKs) can be shared and unencrypted only by those with access to the encrypting root key,
        mitigating the key exchange problem of symmetric algorithms.
- Easier key management
    You can encrypt multiple DEKs under a singular root key, which minimizes the amount of keys that you
    might need to manage in a key management service. You can also choose to save time on key maintenance by only rotating your root keys, instead of
    rotating and re-encrypting all of your DEKs. Note that in cases such as personnel turnover, process malfunctions, or the
    detection of a security issue, it is recommended to rotate all DEKs and root keys associated with the incident.
- Data Key Protection
    Since your DEKs are wrapped by a root key, you do not have to worry about how to store the encrypted data key. Due to this, you
    can store the wDEK with alongside the associated encrypted data.

{{site.data.keyword.keymanagementserviceshort}} uses the Advanced Encryption Standard algorithm in Galois/Counter Mode (AES GCM) to wrap and unwrap DEKs. CRKs that are not imported are created with 256-bit key material. Imported CRKs can be have 128, 192, or 256-bit key material.
{: note}

## How envelope encryption works
{: #envelope-encryption-overview}

Envelope encryption works by using a root key to
wrap (encrypt) one or more data encryption keys (DEKs). The root key safeguards
your wrapped (encrypted) DEKs with encryption algorithms, protecting them from
unauthorized access or exposure.

Unwrapping a DEK reverses the envelope encryption process by using the associated
root key, resulting in a decrypted and authenticated DEK. A wrapped DEK cannot
be unwrapped without the associated root key, which adds an extra layer of
security to your data.

Envelope encryption is treated briefly in the NIST Special Publication 800-57,
Recommendation for Key Management. To learn more, see
[NIST SP 800-57 Pt. 1 Rev. 4.](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}.

## Key types
{: #key-types}

The service supports two key types, root keys and standard keys, for the
advanced encryption and management of data.

Root keys

:   Root keys are primary resources in
   {{site.data.keyword.keymanagementserviceshort}}. They are symmetric
   key-wrapping keys used as roots of trust for wrapping (encrypting) and
   unwrapping (decrypting) other keys stored in a data service. Root keys
   contain key material that is used to wrap and unwrap DEKs. The key
   material of the root key can either be imported or generated by the
   {{site.data.keyword.keymanagementserviceshort}} service.
:   With {{site.data.keyword.keymanagementserviceshort}}, you can create,
   store, and manage the lifecycle of root keys to achieve full control of
   other DEKs stored in the cloud. Unlike a standard key, the plaintext of
   a root key can never leave the bounds of the
   {{site.data.keyword.keymanagementserviceshort}} service.

Standard keys

:   Standard keys, which can be used as DEKs, can store encrypted data in
   {{site.data.keyword.keymanagementserviceshort}}. Once your plaintext
   standard key is generated and protecting underlying data, destroy the
   plaintext and safely store the encrypted standard key.

## Wrapping keys with envelope encryption
{: #wrapping}

You can wrap one or more DEKs with advanced encryption algorithms by
designating a root key in {{site.data.keyword.keymanagementserviceshort}} that
you can fully manage.

Complete the following steps to encrypt data via envelope encryption:

1. Generate or provision a DEK from an IBM Cloud service, such as [COS](/docs/cloud-object-storage?topic=cloud-object-storage-encryption),
    to encrypt your sensitive data. The DEK will encrypt your sensitive data.
2. [Generate](/docs/key-protect?topic=key-protect-create-root-keys)
    or [import](/docs/key-protect?topic=key-protect-import-root-keys)
    a root key that will be used to protect the DEK from step 1.
3. Use the root key to encrypt, or [wrap](/docs/key-protect?topic=key-protect-wrap-keys)
    the DEK. To provide maximum encryption security, you can specify additional
    authenticated data (AAD) during your wrap request. Note that the same
    authentication data will be required when unwrapping the DEK.
4. The encrypted data is then stored as metadata in the {{site.data.keyword.keymanagementserviceshort}}
    service. Store and maintain the base64 encoded output from the wrapped DEK in a secure location,
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
you can use the {{site.data.keyword.keymanagementserviceshort}} API
to send an [unwrap request](/docs/key-protect?topic=key-protect-unwrap-keys){: external} to the service.


Complete the following steps to encrypt data via envelope encryption:

1. Retrieve the base64 encoded ciphertext of the wrapped DEK.
2. [Retrieve the key ID](/docs/key-protect?topic=key-protect-view-keys)
    of the root key that is associated with the DEK.
3. Make an [unwrap request](/docs/key-protect?topic=key-protect-unwrap-keys){: external} to {{site.data.keyword.keymanagementserviceshort}}.
    Note that if you specified additional authenticated data (AAD) in a previous
    wrap request, you must supply it in your unwrap request.
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
