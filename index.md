---

copyright:
  years: 2017, 2025
lastupdated: "2025-06-05"

keywords: key management service, manage encryption keys, data encryption, getting started

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

# Getting started tutorial
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} helps you provision or import encrypted keys for applications for many {{site.data.keyword.cloud_notm}} services that can be managed from a central location. This tutorial shows you how to create and import existing cryptographic keys by using the {{site.data.keyword.keymanagementserviceshort}} dashboard. To find out more about managing and protecting your encryption keys with {{site.data.keyword.keymanagementserviceshort}}, and about relevant use cases, check out [About key protect](/docs/key-protect?topic=key-protect-about).
{: shortdesc}

For a version of this tutorial that talks about the process of creating or importing keys into {{site.data.keyword.keymanagementserviceshort}}, check out [Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).

## Getting started with encryption keys
{: #get-started-keys}

From the {{site.data.keyword.keymanagementserviceshort}} dashboard, you can create new keys or import your existing keys.

Choose from two key types:

* **Root keys**: Symmetric keys that are used to protect other cryptographic keys with [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption) that you fully manage in {{site.data.keyword.keymanagementserviceshort}}.

* **Standard keys**: Symmetric keys that are typically used to directly encrypt and decrypt data such as secrets and passwords.

While many of the topics in this documentation show how to accomplish tasks in both the console and with the APIs, there is also a separate API repo for the {{site.data.keyword.keymanagementserviceshort}} APIs. For that documentation, check out [{{site.data.keyword.keymanagementserviceshort}} API](https://cloud.ibm.com/apidocs/key-protect){: external}.

## Creating new keys
{: #create-keys}

[After you create an instance of {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-provision), you're ready to create keys in the service. In this example, we'll create a root key.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. To create a new key, click **Add**. A side panel will open. Make sure the **Create a key** option is selected.

    Specify the key's details:

| Setting | Description |
| --- | --- |
| Type | The [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. Root keys are selected by default.|
| Key name | A human-readable display name for easy identification of your key. Length must be within 2 - 90 characters (inclusive). To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location. Note that key names do not need to be unique.|
| Key description | **Optional**. Descriptions are a useful way to add information about a key (for example, a phrase describing its purpose) in a way that isn't possible to do using an alias or its name. This description must be at least two characters and no more than 240, and cannot be changed later. To protect your privacy, do not use personal data, such as your name or location, as a description for your key.|
| Key alias | **Optional**. [Key aliases](/docs/key-protect?topic=key-protect-create-key-alias) are ways to describe a key that allow them to be identified and grouped beyond the limits of a display name. Keys can have up to five aliases.|
|Key ring| **Optional**. [Key rings](/docs/key-protect?topic=key-protect-grouping-keys) are groupings of keys that allow those groupings to be managed independently as needed. Every key must be a part of a key ring. If no key ring is selected, keys are placed in the `default` key ring. Note that to place the key you're creating in a key ring, you must have the _Manager_ role over that key ring. For more information about roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).|
|Rotation policy| **Optional**. If you hold the [_Manager_ role](/docs/key-protect?topic=key-protect-manage-access), you can set a rotation policy for the key at key-creation time. If an [instance policy](/docs/key-protect?topic=key-protect-set-rotation-policy) exists to create rotation policies on keys by default, you can also overwrite that policy at key-creation time to a different interval. Note: if you do not posses the _Manager_ role on this instance (or an equivalent level of permissions), this field is not visible.|
{: caption="Describes the Create a key settings." caption-side="bottom"}

When you are finished filling out the key's details, click **Create key** to confirm.

Keys that are created in the service are symmetric 256-bit keys, supported by the AES-CBC-PAD algorithm. For added security, keys are generated by FIPS 140-2 Level 3 certified hardware security modules (HSMs) that are located in secure {{site.data.keyword.cloud_notm}} data centers.

For more information about creating root keys, check out [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys).

For more information about creating standard keys, check out [Creating standard keys](/docs/key-protect?topic=key-protect-create-standard-keys).

## Importing your own keys
{: #import-keys}

You can enable the security benefits of Bring Your Own Key (BYOK) by importing your existing keys to the service. In this example, we'll import a root key.

Complete the following steps to add an existing key.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. To import a key, click **Add** and select the **Import your own key** window.

    Specify the key's details:

|Setting|Description|
| --- | --- |
|Key type|The [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. Select the **Root key** button.|
|Name|A human-readable alias for easy identification of your key. Length must be within 2 - 90 characters (inclusive). <br><br>To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location. Note that key names do not need to be unique.|
|Key material|The base64-encoded key material, such as an existing key-wrapping key, that you want to store and manage in the service. For more information, check out [Base64 encoding your key material](/docs/key-protect?topic=key-protect-import-root-keys#how-to-encode-root-key-material). Ensure that the key material is 16, 24, or 32 bytes long, and corresponds to 128, 192, or 256 bits in length. The key must also be base64-encoded.|
| Key description | **Optional**. Descriptions are a useful way to add information about a key (for example, a phrase describing its purpose) in a way that isn't possible to do using an alias or its name. This description must be at least two characters and no more than 240, and cannot be changed later. To protect your privacy, do not use personal data, such as your name or location, as a description for your key.|
| Key alias | **Optional**. [Key aliases](/docs/key-protect?topic=key-protect-create-key-alias) are ways to describe a key that allow them to be identified and grouped beyond the limits of a display name. Keys can have up to five aliases.|
|Key ring| **Optional**. [Key rings](/docs/key-protect?topic=key-protect-grouping-keys) are groupings of keys that allow those groupings to be managed independently as needed. Every key must be a part of a key ring. If no key ring is selected, keys are placed in the `default` key ring. Note that to place the key you're creating in a key ring, you must have the _Manager_ role over that key ring. For more information about roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).|
{: caption="Describes the Import your own key settings." caption-side="top"}

Rotation policies cannot be applied to a key when importing a key. Imported keys must be [rotated manually](/docs/key-protect?topic=key-protect-rotate-keys).

When you are finished filling out the key's details, click **Import key** to confirm.

From the {{site.data.keyword.keymanagementserviceshort}} dashboard, you can inspect the general characteristics of your new keys.

You can programmatically enable an extra layer of protection to Bring Your Own Key (BYOK) by encrypting your key material before you import a key into {{site.data.keyword.keymanagementserviceshort}}.

For more information about importing root keys, check out [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys).

For more information about importing standard keys, check out [Importing standard keys](/docs/key-protect?topic=key-protect-import-standard-keys).

## Using Key Protect
{: #get-started-next-steps}

- If you added a root key to the service, learn more about using the root key to protect the keys that encrypt your data at rest by checking out [Wrapping keys](/docs/key-protect?topic=key-protect-wrap-keys).

- To find out more about integrating the {{site.data.keyword.keymanagementserviceshort}} service with other cloud data solutions, check out the [Integrations](/docs/key-protect?topic=key-protect-integrate-services) documentation.

- To find out more about programmatically managing your keys, check out the [{{site.data.keyword.keymanagementserviceshort}} API reference](/apidocs/key-protect){: external} documentation.

### Best practices for using Key Protect
{: #get-started-next-steps-best-practices}

While the best way to use {{site.data.keyword.keymanagementserviceshort}} ultimately depends on the needs of your use case, there are a number of best practices to keep in mind that are specific to {{site.data.keyword.keymanagementserviceshort}}. These recommendations are in addition to the general best practices that should be observed when using any IBM Cloud or IBM Cloud Object Storage service.

### Using a secure backup
{: #get-started-next-steps-best-practices-secure-backup}

If you import a root key into {{site.data.keyword.keymanagementserviceshort}}, you are encouraged to maintain a secure backup of the key material. This key material will allow you to generate an equivalent key in the event that, for example, you accidentally delete your key. Deleted keys can be [restored within 30 days](/docs/key-protect?topic=key-protect-delete-purge-keys) of being deleted, but if 30 days have passed, you can use stored key material to create a functionally identical key (it will be able to unwrap data encryption keys created by the deleted root key, for example). You will need to update your application to use the unique key ID given for this functionally identical key and to change the endpoint (if the new key is created in a different region).

Because any key that uses the same material during key creation is functionally equivalent, you are encouraged to keep your backed up key material secure.
{: tip}

Similarly, you can also securely backup your root keys by using the key material to create a duplicate key in a {{site.data.keyword.keymanagementserviceshort}} region that is different from the original key.

Every time a root key is rotated, new key material is added to the key, which creates a new version of the key. As a result, your equivalent keys (duplicate keys created using the same key material) should be kept up to date with the new material.
{: note}

### Using key aliases with duplicate keys
{: #get-started-next-steps-best-practices-key-aliases}

Because keys created with the same material are functionally identical (that is, both can be used to wrap and unwrap the same data), users have the option of creating these keys in different regions and using them as backups in the event a data center is temporarily unavailable. While these duplicates will have different key IDs, they can be given the same [key alias](/docs/key-protect?topic=key-protect-create-key-alias). This alias can serve to organize functionally identical keys together, making it easier to update an application to point to these backup keys.

### Using key rings
{: #get-started-next-steps-best-practices-key-rings}

[Key rings](/docs/key-protect?topic=key-protect-grouping-keys) are a way to create a group of keys for a target group of users that require the same IAM access permissions. This allows an account admin to easily tailor the keys they own to the users who manage them and is therefore less error prone than manually updating the permissions of each key and user.

### Rotating your keys
{: #get-started-next-steps-best-practices-key-rotate}

You should [rotate your root keys](/docs/key-protect?topic=key-protect-key-rotation) (that is, to create a new version of the key) on a regular basis. Regular rotations reduce what is known as the "cryptoperiod" of the key, and can also be used in specific cases such as personnel turnover, process malfunctions, or the detection of a security issue.

Root keys can be rotated manually or, if the key was created using {{site.data.keyword.keymanagementserviceshort}}, on a schedule set by the owner of the key. The [option you choose](/docs/key-protect?topic=key-protect-set-rotation-policy) depends on your preferences and the needs of your use case.

For more information about rotating keys, check out [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

### Creating your own key material
{: #get-started-next-steps-best-practices-key-material}

While it is comparatively simple to [Base64-encode key material](/docs/key-protect?topic=key-protect-import-root-keys#how-to-encode-root-key-material), it is important to follow NIST guidelines when creating your own key material. Improperly created key material can make your keys more susceptible to being compromised. Unless you feel confident in creating the appropriate key material yourself, the best practice is to let {{site.data.keyword.keymanagementserviceshort}} create your key material for you as part of the key creation process, which follows the latest NIST guidelines.

Because this key material can be used to [create a functionally duplicate key](#get-started-next-steps-best-practices-key-aliases), whether you have created the key material yourself or exported a {{site.data.keyword.keymanagementserviceshort}} created key, make sure you keep this key material secure.
