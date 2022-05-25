---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-25"

keywords: import encryption key, Bring Your Own Key, BYOK, upload key

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
{:preview: .preview}
{:term: .term}

# Bringing your encryption keys to the cloud
{: #importing-keys}

Encryption keys contain subsets of information, such as the metadata that helps
you identify the key, and the _key material_ that's used to encrypt and decrypt
data.
{: shortdesc}

When you use {{site.data.keyword.keymanagementserviceshort}} to create keys, the
service generates cryptographic key material on your behalf that's rooted in
cloud-based hardware security modules (HSMs). But depending on your business
requirements, you might need to generate key material from your internal
solution, and then extend your on-premises key management infrastructure onto
the cloud by importing keys into
{{site.data.keyword.keymanagementserviceshort}}.

| Benefit | Description |
| ------- | ----------- |
| Bring your own keys (BYOK) | You want to fully control and strengthen your key management practices by generating strong keys from your on-premises hardware security module (HSM). If you choose to export symmetric keys from your internal key management infrastructure, you can use {{site.data.keyword.keymanagementserviceshort}} to securely bring them to the cloud. |
| Secure import of root key material | When you export your keys to the cloud, you want assurance that the key material is protected while it's in flight. Mitigate against man-in-the-middle attacks by [using an import token](#using-import-tokens) to securely import root key material into your {{site.data.keyword.keymanagementserviceshort}} instance. |
{: caption="Table 1. Describes the benefits of importing key material" caption-side="top"}

Imported keys cannot be [scheduled for automatic rotation](/docs/key-protect?topic=key-protect-key-rotation). They must be rotated manually.
{: note}

## Planning ahead for importing key material
{: #plan-ahead}

Keep the following considerations in mind when you're ready to import root key
material to the service.

### Review your options for creating key material
{: #importing-keys-plan-ahead-1}

Explore your options for creating 256-bit symmetric encryption keys based on
your security needs.

For example, you can use your internal key management system, backed by a
FIPS-validated, on-premises hardware security module (HSM), to generate key
material before you bring keys to the cloud.

If you're building a proof of concept, you can also use a cryptography toolkit
such as
[OpenSSL](https://www.openssl.org/){: external}
to generate key material that you can import into
{{site.data.keyword.keymanagementserviceshort}} for your testing needs.

### Choose an option for importing key material into {{site.data.keyword.keymanagementserviceshort}}
{: #importing-keys-plan-ahead-2}

Choose from two options for importing root keys based on the level of
security that's required for your environment or workload.

By default, {{site.data.keyword.keymanagementserviceshort}} encrypts your key material while it's in transit by using supported ciphers of the Transport Layer Security (TLS) 1.2 and 1.3 protocols. For more information about those ciphers, check out [Data encryption](/docs/key-protect?topic=key-protect-security-and-compliance#data-security).

If you're building a proof of concept or trying out the service for the
first time, you can import root key material into
{{site.data.keyword.keymanagementserviceshort}} by using this default
option.

If your workload requires a security mechanism beyond TLS, you can also
[use an import token](#using-import-tokens)
to encrypt and import root key material into the service.

### Plan ahead for encrypting your key material
{: #importing-keys-plan-ahead-3}

If you choose to encrypt your key material by using an import token, determine a
method for running RSA encryption on the key material. You must use the
`RSAES_OAEP_SHA_256` encryption scheme as specified by the
[PKCS #1 v2.1 standard for RSA encryption](https://tools.ietf.org/html/rfc3447){: external}.

Review the capabilities of your internal key management system or on-premises
HSM to determine your options, or check out the
[secure import tutorial](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-key)
for examples.

### Plan ahead for encrypting the nonce
{: #importing-keys-plan-ahead-4}

If you choose to encrypt your key material by using an import token, you must
also determine a method for running AES-GCM encryption on the nonce that is
distributed by {{site.data.keyword.keymanagementserviceshort}}.

The nonce serves as a session token that checks the originality of a request to
protect against malicious attacks and unauthorized calls.

Review the capabilities of your internal key management system or
on-premises HSM to determine your options, or check out the
[secure import tutorial](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce)
for examples.

### Manage the lifecycle of imported key material
{: #importing-keys-plan-ahead-5}

After you import key material into the service, keep in mind that you are
responsible for managing the complete lifecycle of your key. By using the
{{site.data.keyword.keymanagementserviceshort}} API, you can set an expiration
date for the key when you decide to upload it into the service.

However, if you want to
[rotate an imported root key](/docs/key-protect?topic=key-protect-rotate-keys),
you must generate and provide new key material to retire and replace the
existing key.

## Using import tokens
{: #using-import-tokens}

If you want to encrypt your key material before you import it into
{{site.data.keyword.keymanagementserviceshort}}, you can create an import token
for your {{site.data.keyword.keymanagementserviceshort}} instance by using the
{{site.data.keyword.keymanagementserviceshort}} API.

Import tokens are a resource type in
{{site.data.keyword.keymanagementserviceshort}} that enable the secure import of
key material to your {{site.data.keyword.keymanagementserviceshort}} instance.
By using the contents of an import token to encrypt your key material
on-premises, you protect root keys while they're in flight to
{{site.data.keyword.keymanagementserviceshort}} based on the policies that you
specify. For example, you can set a policy on the import token that limits its
use based on time and usage count.

### How it works
{: #how-import-tokens-work}

When you
[create an import token](/docs/key-protect?topic=key-protect-create-import-tokens)
for your {{site.data.keyword.keymanagementserviceshort}} instance,
{{site.data.keyword.keymanagementserviceshort}} generates a 4096-bit RSA
key-pair from its HSMs.

When you
[retrieve the import token](/docs/key-protect?topic=key-protect-create-import-tokens#retrieve-import-token-api),
the service supplies the public key that you can use for encrypting and
uploading a key to {{site.data.keyword.keymanagementserviceshort}}.

The following list describes the import token workflow.

1. **You send a request to create an import token.**

    1. {{site.data.keyword.keymanagementserviceshort}} generates an RSA key-pair
        from its HSMs.

    2. The public key becomes available for retrieval based on the policy that
        you specified at creation time.

    3. The private key becomes non-extractable and never leaves the HSM.

2. **You send a request to retrieve the import token.**

    1. You receive the import token contents, including:

        - A public key for the encrypting key material that you want to import
        into the service.

        - A nonce value that's used to verify the key import request.

3. **You prepare the key that you want to import to the service.**

    1. You generate key material by using an on-premises key management
        mechanism.

    2. You encrypt the nonce value with the key material by using an AES-GCM
        encryption method that is compatible with your environment.

    3. You encrypt the key material with the public key by using an RSA
        encryption method that is compatible with your environment.

4. **You send a request to import the key.**

    1. You provide the encrypted key material, the encrypted nonce, and the
        initialization vector (IV) that was generated by the AES-GCM algorithm.

    2. {{site.data.keyword.keymanagementserviceshort}} verifies your request,
        decrypts the encrypted packet, and stores the key material as a root key
        in your {{site.data.keyword.keymanagementserviceshort}} instance.

You can create only one import token per
{{site.data.keyword.keymanagementserviceshort}} instance at a time. To learn
more about retrieval limits for import tokens,
[see the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect#create-an-import-token){: external}.
{: note}

To try out the import token feature, see
[Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).
{: tip}

### API methods
{: #secure-import-api-methods}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API
drives the import token creation process.

The following table lists the API methods that set up an import token for your
{{site.data.keyword.keymanagementserviceshort}} instance.

| Method | Description |
| ------ | ----------- |
| `POST api/v2/import_token` | [Create an import token](/docs/key-protect?topic=key-protect-create-import-tokens) |
| `GET api/v2/import_token` | [Retrieve an import token](/docs/key-protect?topic=key-protect-create-import-tokens) |
{: caption="Table 2. Describes the {{site.data.keyword.keymanagementserviceshort}} API methods" caption-side="top"}

To find out more about programmatically managing your keys in
{{site.data.keyword.keymanagementserviceshort}}, check out the
[{{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.

## What's next
{: #importing-keys-whats-next}

- To learn how to create an import token for your
    {{site.data.keyword.keymanagementserviceshort}} instance, see
    [Creating an import token](/docs/key-protect?topic=key-protect-create-import-tokens).

- To find out more about importing keys to the service, see
    [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys).
