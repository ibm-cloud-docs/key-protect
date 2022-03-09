---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-09"

keywords: import standard encryption key, import secret, upload secret

subcollection: key-protect

---

{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Importing standard keys
{: #import-standard-keys}

You can add your existing encryption keys by using the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

{: ui}

You can add your existing encryption keys programmatically with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

{: api}

## Importing standard keys with the console
{: #import-standard-key-gui}
{: ui}

[After you create an instance of the service](/docs/key-protect?topic=key-protect-provision), complete the following steps to import an existing key by using the {{site.data.keyword.cloud_notm}} console.

If you enable [dual authorization settings for your {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth), keep in mind that any keys that you add to the service require an authorization from two users to delete keys.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. To import a new key, click **Add key** and select the **Import your own key**
    window.

    Specify the key's details:

|Setting|Description|
|--- |--- |
|Key type|The [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. Click the **Standard key** button.|
|Name|A human-readable alias for easy identification of your key. Length must be within 2 - 90 characters (inclusive). <br><br>To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location. Note that key names do not need to be unique.|
|Key material|The base64-encoded key material, such as an existing key-wrapping key, that you want to store and manage in the service. For more information, check out [Base64 encoding your key material](#how-to-encode-standard-key-material). Ensure that the key material is 16, 24, or 32 bytes long, and corresponds to 128, 192, or 256 bits in length. The key must also be base64-encoded.|
| Key alias | **Optional**. [Key aliases](/docs/key-protect?topic=key-protect-create-key-alias) are ways to describe a key that allow them to be identified and grouped beyond the limits of a display name. Keys can have up to five aliases.|
|Key ring| **Optional**. [Key rings](/docs/key-protect?topic=key-protect-grouping-keys) are groupings of keys that allow those groupings to be managed independently as needed. Every key must must be a part of a key ring. If no key ring is selected, keys are placed in the `default` key ring. Note that to place the key you're creating in a key ring, you must have the _Manager_ role over that key ring. For more information about roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).|
{: caption="Table 1. Describes the Import your own key settings." caption-side="top"}

When you are finished filling out the key's details, click **Import key** to confirm.

If you know which key ring you want a key to be placed in, and you are a _Manager_ of that key ring, you can also navigate to the **Key rings** panel, select ⋯ and click **Add key to key ring**. This will open the same panel you see by clicking **Add** on the **Keys** page with the **Key rings** variable filled in with the name of the key ring.
{: tip}

## Importing standard keys
{: #import-standard-key-api}
{: api}

Import a standard key by making a `POST` call to the following endpoint:

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Call the [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external} with the following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
        -H "correlation-id: <correlation_ID>" \
        -H "prefer: <return_preference>" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.key+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.key+json",
                        "name": "<key_name>",
                        "aliases": [alias_list],
                        "description": "<key_description>",
                        "expirationDate": "<expiration_date>",
                        "payload": "<key_material>",
                        "extractable": <key_type>
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|The unique identifier that is used to track and correlate transactions.|
|return_preference|A header that alters server behavior for POST and DELETE operations.<br><br>When you set the return_preference variable to return=minimal, the service returns only the key metadata, such as the key name and ID value, in the response entity-body. When you set the variable to return=representation, the service returns both the key material and the key metadata.|
|key_name|**Required**. A unique, human-readable name for easy identification of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|alias_list|**Optional**.One or more unique, human-readable aliases assigned to your key.<br><br>Important: To protect your privacy, do not store your personal data as metadata for your key.<br><br>Each alias must be alphanumeric, case sensitive, and cannot contain spaces or special characters other than - or _. The alias cannot be a UUID and must not be a {{site.data.keyword.keymanagementserviceshort}} reserved name: allowed_ip, key, keys, metadata, policy, policies. registration, registrations, ring, rings, rotate, wrap, unwrap, rewrap, version, versions.|
|key_description|**Optional**.An extended description of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|expiration_date|**Optional**.The date and time that the key expires in the system, in RFC 3339 format (YYYY-MM-DD HH:MM:SS.SS, for example 2019-10-12T07:20:50.52Z). The key will transition to the deactivated state within one hour past the key's expiration date. If the expirationDate attribute is omitted, the key does not expire.|
|key_material|**Required**.The base64-encoded key material, such as a symmetric key, that you want to manage in the service. For more information, check out [Base64 encoding your key material](#how-to-encode-standard-key-material).<br><br>Ensure that the key material meets the following requirements:<br>A standard key can be up to 7,500 bytes in size. The key must be base64-encoded.|
|key_type|A boolean value that determines whether the key material can leave the service.<br><br>When you set the extractable attribute to `true`, the service designates the key as a standard key that you can store in your apps or services.|
{: caption="Table 1. Describes the variables that are needed to add a standard key with the {{site.data.keyword.keymanagementserviceshort}} API" caption-side="top"}

To protect the confidentiality of your personal data, avoid entering
personally identifiable information (PII), such as your name or location,
when you add keys to the service. For more examples of PII, see section 2.2
of the
[NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

A successful `POST api/v2/keys` response returns the ID value for your key,
along with other metadata. The ID is a unique identifier that is assigned to
your key and is used for subsequent calls to the
{{site.data.keyword.keymanagementserviceshort}} API.

### Optional: Verify standard key importation
{: #import-standard-key-api-verify}

You can verify that a standard key has been imported by issuing a list keys request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance, and your `<IAM_token>` is your IAM token.

## Base64-encoding your key material
{: #how-to-encode-standard-key-material}

When importing an existing standard key, it is required to include the encrypted key material that you want to store and manage in the service.

### Using OpenSSL to encrypt existing key material
{: #open-ssl-encoding-standard-existing-key-material}

Use this process to encrypt the contents of a file. For example, you might have a file with credentials, not just an encrypted key, that you want to store in {{site.data.keyword.keymanagementserviceshort}}.

1. Download and install
    [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.

2. Base64-encode your key material string by running the following command:

    ```sh
    openssl base64 -in <infile> -out <outfile>
    ```
    {: pre}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|infile|The name of the file where your key material string resides.<br><br>Ensure that the key is 16, 24, or 32 bytes long, corresponding to 128, 192, or 256 bits in length. The key must be base64-encoded.|
|outfile|The name of the file where your base64-encoded key material will be created once the command has run.|
{: caption="Table 2. Describes the variables that are needed to base64-encode your key material." caption-side="top"}

If you want to output the base64 material in the command line directly rather
than a file, run the command
`openssl enc -base64 <<< '<key_material_string>'`, where key_material_string
is the key material input for your imported key.
{: note}

### Using OpenSSL to create and encode new key material
{: #open-ssl-encoding-standard-new-key-material}

Use this process to create a random base64-encoded key material with a specific byte length. 32 bytes (256 bits) is recommended.

You would create a 16-, 24-, or 32-byte key material, for use as a standard key, if you want the standard key to have the same characteristics as a root key. A standard key is one which can leave the service. Standard keys are often used in your apps and services.
{: note}

1. Download and install
    [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.

2. Base64-encode your key material string by running the following command:

    ```sh
    openssl rand -base64 <byte_length>
    ```
    {: pre}

    Replace the variable in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|byte_length|The length of the key, measured in bytes.<br><br>Acceptable byte lengths are 16, 24, or 32 bytes, corresponding to 128, 192, or 256 bits in length. The key must be base64-encoded.|
{: caption="Table 3. Describes the variable that is needed to create and encode new key material." caption-side="top"}

#### Key Material Creation Examples
{: #import-standard-key-open-ssl-examples}

1. `openssl rand -base64 16` will generate a 128-bit key material.

2. `openssl rand -base64 24` will generate a 192-bit key material.

3. `openssl rand -base64 32` will generate a 256-bit key material.

## What's next
{: #import-standard-key-next-steps}

- To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.


