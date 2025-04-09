---

copyright:
  years: 2017, 2025
lastupdated: "2025-04-09"

keywords: create root key, create key-wrapping key, create CRK

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
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Creating root keys
{: #create-root-keys}

Use {{site.data.keyword.keymanagementservicefull}} to create root keys with the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

{: ui}

Use {{site.data.keyword.keymanagementservicefull}} to create root keys programmatically with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

{: api}

Root keys are symmetric key-wrapping keys that are used to protect the security of encrypted data in the cloud. For more information about root keys, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

Encryption keys that are created in one region can be used to encrypt data stores located in any region within IBM Cloud.
{: note}

You can specify an expiration date when creating a root key. After performing the wrap, unwrap, rewrap, get key, or get key metadata actions on a key, an associated {{site.data.keyword.logs_full_notm}} event will send information on the date that the key expires and how many days are left until that day arrives.

## Creating root keys in the console
{: #create-root-key-gui}
{: ui}

[After you create an instance of the service](/docs/key-protect?topic=key-protect-provision), complete the following steps to create a root key in the
{{site.data.keyword.cloud_notm}} console.

If you enable [dual authorization settings for your {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth), keep in mind that any keys that you add to the service require an authorization from two users to delete keys.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. To create a new key, click **Add**. A side panel will open. Make sure the **Create a key** option is selected. Note that to set a **Key alias**, **Key Ring**, or **Rotation policy** for this key, you must click the **Advanced Options** tab to reveal them.

If you are not a `Manager` (or have an equivalent level of permissions), the **Rotation policy** option does not appear.
{: note}

    Specify the key's details:

| Setting | Description |
| --- | --- |
| Type | The [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. Root keys are selected by default.|
| Key name | A human-readable display name for easy identification of your key. Length must be within 2 - 90 characters (inclusive). To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location. Note that key names do not need to be unique.|
| Key description | **Optional**. Descriptions are a useful way to add information about a key (for example, a phrase describing its purpose) in a way that isn't possible to do using an alias or its name. This description must be at least two characters and no more than 240, and cannot be changed later. To protect your privacy, do not use personal data, such as your name or location, as a description for your key.|
| Key alias | **Optional**. A [key alias](/docs/key-protect?topic=key-protect-create-key-alias) is also a way to describe a key. Keys can have up to five aliases.|
|Key ring| **Optional**. [Key rings](/docs/key-protect?topic=key-protect-grouping-keys) are groupings of keys that allow those groupings to be managed independently as needed. Every key must be a part of a key ring. If no key ring is selected, keys are placed in the `default` key ring. Note that to place the key you're creating in a key ring, you must have the _Manager_ role over that key ring. For more information about roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).|
|Rotation policy| **Optional**. If you hold the [_Manager_ role](/docs/key-protect?topic=key-protect-manage-access), you can set a rotation policy for the key at key-creation time. If an [instance policy](/docs/key-protect?topic=key-protect-set-rotation-policy) exists to create rotation policies on keys by default, you can also overwrite that policy at key-creation time to a different interval. Note that if your instance has a rotation policy enabled and you **Disable** the rotation policy at key creation time, the policy is still written to your key in a **Disabled** state. If you want to enable this policy later, you can do so. Check out [Set a rotation policy after the key has been created](/docs/key-protect?topic=key-protect-set-rotation-policy#manage-policies-gui) for more information.|
{: caption="Describes the Create a key settings." caption-side="bottom"}

When you are finished filling out the key's details, click **Create key** to confirm.

If you know which key ring you want a key to be placed in, and you are a _Manager_ of that key ring, you can also navigate to the **Key rings** panel, select ⋯ and click **Add key to key ring**. This will open the same panel you see by clicking **Add** on the **Keys** page with the **Key rings** variable filled in with the name of the key ring.
{: tip}

Keys that are created in the service are symmetric 256-bit keys, supported by the AES-CBC-PAD algorithm. For added security, keys are generated by FIPS 140-2 Level 3 certified hardware security modules (HSMs) that are located in secure {{site.data.keyword.cloud_notm}} data centers.

## Creating root keys with the API
{: #create-root-key-api}
{: api}

Create a root key by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a root key by running the following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "correlation-id: <correlation_ID>" \
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
                        "extractable": <key_type>
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

| Variable | Description |
| --- | --- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the target key ring that you would like the newly create key to be a part of. If unspecified, the header is automatically set to 'default' and the key will sit in the default key ring in the specified {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|The unique identifier that is used to track and correlate transactions.|
|key_name|**Required**. A human-readable name for convenient identification of your key. Important: To protect your privacy, do not store your personal data as metadata for your key.|
|alias_list|One or more unique, human-readable aliases assigned to your key. Important: To protect your privacy, do not store your personal data as metadata for your key. Each alias must be alphanumeric, case sensitive, and cannot contain spaces or special characters other than dashes (-) or underscores (_). The alias cannot be a version 4 UUID and must not be a {{site.data.keyword.keymanagementserviceshort}} reserved name: allowed_ip, key, keys, metadata, policy, policies, registration, registrations, ring, rings, rotate, wrap, unwrap, rewrap, version, versions. Alias size can be between 2 - 90 characters (inclusive).|
|key_description|An extended description of your key. Important: To protect your privacy, do not store your personal data as metadata for your key.|
|expiration_date|**Optional**. The date and time that the key expires in the system, in RFC 3339 format (`YYYY-MM-DD HH:MM:SS.SS`, for example `2019-10-12T07:20:50.52Z`). The key will transition to the deactivated state within one hour past the key's expiration date. If the expirationDate attribute is omitted, the key does not expire.|
|key_type|A boolean value that determines whether the key material can leave the service. When you set the extractable attribute to false, the service creates a root key that you can use for wrap or unwrap operations.|
{: caption="Describes the variables that are needed to add a root key with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="bottom"}

To protect the confidentiality of your personal data, avoid entering personally identifiable information (PII), such as your name or location, when you add keys to the service.
{: important}

If the `expirationDate` is provided in your create key request, the key will transition to the deactivated state within one hour past the key's expiration date.
{: note}

A successful `POST api/v2/keys` response returns the ID value for your key, along with other metadata. The ID is a unique identifier that is assigned to your key and is used for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "type": "application/vnd.ibm.kms.key+json",
            "id": "02fd6835-6001-4482-a892-13bd2085f75d",
            "name": "test-root-key",
            "aliases": [
                "alias-1",
                "alias-2"
              ],
            "description": "A test root key",
            "state": 1,
            "extractable": false,
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "imported": false,
            "creationDate": "2020-03-12T03:37:32Z",
            "createdBy": "...",
            "algorithmType": "Deprecated",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "Deprecated"
            },
            "algorithmBitSize": 256,
            "algorithmMode": "Deprecated",
            "lastUpdateDate": "2020-03-12T03:37:32Z",
            "keyVersion": {
                "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
                "creationDate": "2020-03-12T03:37:32Z"
            },
            "dualAuthDelete": {
                "enabled": false
            },
            "deleted": false
        }
    ]
}
```
{: screen}

For a detailed description of the response parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](/apidocs/key-protect){: external}.
{: tip}

## What's next
{: #create-root-key-next-steps}

- To find out more about protecting keys with envelope encryption, check out [Wrapping keys](/docs/key-protect?topic=key-protect-wrap-keys).

- To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
