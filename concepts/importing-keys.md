---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# Bringing your encryption keys to the cloud
{: #importing-keys}

Encryption keys contain subsets of information, such as the metadata that helps you identify the key, and the _key material_ that's used to encrypt and decrypt data.
{: shortdesc}

When you use {{site.data.keyword.keymanagementserviceshort}} to create keys, the service generates cryptographic key material on your behalf that's rooted in cloud-based hardware security modules (HSMs). But depending on your business requirements, you might need to generate key material from your internal solution, and then extend your on-premises key management infrastructure onto the cloud by importing keys into {{site.data.keyword.keymanagementserviceshort}}.

<table>
  <th>Benefit</th>
  <th>Description</th>
  <tr>
    <td>Bring your own keys (BYOK) </td>
    <td>You want to fully control and strengthen your key management practices by generating strong keys from your on-premises hardware security module (HSM). If you choose to export symmetric keys from your internal key management infrastructure, you can use {{site.data.keyword.keymanagementserviceshort}} to securely bring them to the cloud.</td>
  </tr>
  <tr>
    <td>Secure import of root key material</td>
    <td>When you export your keys to the cloud, you want assurance that the key material is protected while it's in flight. Mitigate against man-in-the-middle attacks by <a href="#transport-keys">using a transport key</a> to securely import root key material into your {{site.data.keyword.keymanagementserviceshort}} service instance.</td>
  </tr>
  <caption style="caption-side:bottom;">Table 1. Describes the benefits of importing key material</caption>
</table>


## Planning ahead for importing key material
{: #plan-ahead}

Keep the following considerations in mind when you're ready to import root key material to the service.

<dl>
  <dt>Review your options for creating key material</dt>
    <dd>Explore your options for creating 256-bit symmetric encryption keys based on your security needs. For example, you can use your internal key management system, backed by a FIPS-validated, on-premises hardware security module (HSM), to generate key material before you bring keys to the cloud. If you're building a proof of concept, you can also use a cryptography toolkit such as <a href="https://www.openssl.org/" target="_blank">OpenSSL</a> to generate key material that you can import into {{site.data.keyword.keymanagementserviceshort}} for your testing needs.</dd>
  <dt>Choose an option for importing key material into {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>Choose from two options for importing root keys based on the level of security that's required for your environment or workload. By default, {{site.data.keyword.keymanagementserviceshort}} encrypts your key material while it's in transit by using the Transport Layer Security (TLS) 1.2 protocol. If you're building a proof of concept or trying out the service for the first time, you can import root key material into {{site.data.keyword.keymanagementserviceshort}} by using this default option. If your workload requires a security mechanism beyond TLS, you can also <a href="#transport-keys">use a transport key</a> to encrypt and import root key material into the service.</dd>
  <dt>Plan ahead for encrypting your key material</dt>
    <dd>If you choose to encrypt your key material by using a transport key, determine a method for running RSA encryption on the key material. You must use the <code>RSAES_OAEP_SHA_256</code> encryption scheme as specified by the <a href="https://tools.ietf.org/html/rfc3447" target="_blank">PKCS #1 v2.1 standard for RSA encryption</a>. Review the capabilities of your internal key management system or on-premises HSM to determine your options.</dd>
  <dt>Manage the lifecycle of imported key material</dt>
    <dd>After you import key material into the service, keep in mind that you are responsible for managing the complete lifecycle of your key. By using the {{site.data.keyword.keymanagementserviceshort}} API, you can set an expiration date for the key when you decide to upload it into the service. However, if you want to <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">rotate an imported root key</a>, you must generate and provide new key material to retire and replace the existing key. </dd>
</dl>

## Using transport keys
{: #transport-keys}

Transport keys are currently a beta feature. Beta features can change at any time, and future updates might introduce changes are incompatible with the latest version.
{: important}

If you want to encrypt your key material before you import it into {{site.data.keyword.keymanagementserviceshort}}, you can create a transport encryption key for your service instance by using the {{site.data.keyword.keymanagementserviceshort}} API. 

Transport keys are a resource type in {{site.data.keyword.keymanagementserviceshort}} that enable the secure import of root key material to your service instance. By using a transport key to encrypt your key material on-premises, you protect root keys while they're in flight to {{site.data.keyword.keymanagementserviceshort}} based on the policies that you specify. For example, you can set a policy on the transport key that limits the use of the key based on time and usage count.

### How it works
{: #how-transport-keys-work}

When you [create a transport key](/docs/services/key-protect?topic=key-protect-create-transport-keys) for your service instance, {{site.data.keyword.keymanagementserviceshort}} generates a 4096-bit RSA key. The service encrypts the private key, and then returns the public key and an import token that you can use for encrypting and importing your root key material. 

When you're ready to [import a root key](/docs/services/key-protect?topic=key-protect-import-root-keys#api) into {{site.data.keyword.keymanagementserviceshort}}, you provide the encrypted root key material and the import token that's used to verify the integrity of the public key. To complete the request, {{site.data.keyword.keymanagementserviceshort}} uses the private key that's associated with your service instance to decrypt the encrypted root key material. This process ensures that only the transport key that you generated in {{site.data.keyword.keymanagementserviceshort}} can decrypt the key material that you import and store in the service.

You can create only one transport key per service instance. To learn more about retrieval limits for transport keys, [see the {{site.data.keyword.keymanagementserviceshort}} API reference doc ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### API methods
{: #secure-import-api-methods}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API drives the transport key creation process.  

The following table lists the API methods that set up a locker and create transport keys for your service instance.

<table>
  <tr>
    <th>Method</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Create a transport key</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Retrieve transport key metadata</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Retrieve a transport key and an import token</a></td>
  </tr>
  <caption style="caption-side:bottom;">Table 2. Describes the {{site.data.keyword.keymanagementserviceshort}} API methods</caption>
</table>

To find out more about programmatically managing your keys in {{site.data.keyword.keymanagementserviceshort}}, check out the [{{site.data.keyword.keymanagementserviceshort}} API reference doc ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.

## What's next

- To learn how to create a transport key for your service instance, see [Creating a transport key](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- To find out more about importing keys to the service, see [Importing root keys](/docs/services/key-protect?topic=key-protect-import-root-keys). 