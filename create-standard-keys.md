---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-02"

keywords: create standard encryption key, create secret, standard encryption key API examples

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

# Creating standard keys
{: #create-standard-keys}

You can create a standard encryption key with the {{site.data.keyword.keymanagementserviceshort}} console.
{: shortdesc}
{: ui}

You can create a standard encryption key programmatically with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}
{: api}

## Creating standard keys in the console
{: #create-standard-key-gui}
{: ui}

[After you create an instance of the service](/docs/key-protect?topic=key-protect-provision), complete the following steps to create a standard key in the {{site.data.keyword.cloud_notm}} console.

If you enable [dual authorization settings for your {{site.data.keyword.keymanagementserviceshort}} instance](/docs/key-protect?topic=key-protect-manage-dual-auth), keep in mind that any keys that you add to the service require an authorization from two users to delete keys.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. To create a new key, click **Add** and leave the **Create a key** option selected.

    Specify the key's details:

| Setting | Description |
| --- | --- |
| Type | The [type of key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that you would like to manage in {{site.data.keyword.keymanagementserviceshort}}. Root keys are selected by default. Select the **Standard key** button to create a standard key.|
| Key name | A human-readable display name for easy identification of your key. Length must be within 2 - 90 characters (inclusive). To protect your privacy, ensure that the key name does not contain personally identifiable information (PII), such as your name or location. Note that key names do not need to be unique.|
| Key alias | **Optional**. [Key aliases](/docs/key-protect?topic=key-protect-create-key-alias) are ways to describe a key that allow them to be identified and grouped beyond the limits of a display name. Keys can have up to five aliases.|
|Key ring| **Optional**. [Key rings](/docs/key-protect?topic=key-protect-key-rings) are groupings of keys that allow those groupings to be managed independently as needed. Every key must be a part of a key ring. If no key ring is selected, keys are placed in the `default` key ring. Note that to place the key you're creating in a key ring, you must have the _Manager_ role over that key ring. For more information about roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).|
{: caption="Table 1. Describes the **Create a key** settings." caption-side="bottom"}

When you are finished filling out the key's details, click **Create key** to confirm.

If you know which key ring you want a key to be placed in, and you are a _Manager_ of that key ring, you can also navigate to the **Key rings** panel, select ⋯ and click **Add key to key ring**. This will open the same panel you see by clicking **Add** on the **Keys** page with the **Key rings** variable filled in with the name of the key ring.
{: tip}

## Creating standard keys with the API
{: #create-standard-key-api}
{: api}

Create a standard key by making a `POST` call to the following endpoint.

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
        -H "x-kms-key-ring: <key_ring_ID" \
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
                        "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
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
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the target key ring that you would like the newly create key to be a part of. If unspecified, the header is automatically set to 'default' and the key will sit in the default key ring in the specified {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|return_preference|A header that alters server behavior for POST and DELETE operations. When you set the `return_preference` variable to `return=minimal`, the service returns only the key metadata, such as the key name and ID value, in the response `entity-body`. When you set the variable to `return=representation`, the service returns both the key material and the key metadata.|
|key_name|**Required**. A human-readable name for convenient identification of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|alias_list|**Optional**.One or more unique, human-readable aliases assigned to your key. Important: To protect your privacy, do not store your personal data as metadata for your key. Each alias must be alphanumeric, case sensitive, and cannot contain spaces or special characters other than `-` or `_`. The alias cannot be a UUID and must not be a {{site.data.keyword.keymanagementserviceshort}} reserved name: allowed_ip, key, keys, metadata, policy, policies, registration, registrations, ring, rings, rotate, wrap, unwrap, rewrap, version, versions. Alias size can be between 2 - 90 characters (inclusive).|
|key_description|**Optional**.An extended description of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|YYYY-MM-DD HH:MM:SS.SS|**Optional**.The date and time that the key expires in the system, in RFC 3339 format. If the expirationDate attribute is omitted, the key does not expire.|
|key_type|**Optional**.A boolean value that determines whether the key material can leave the service. When you set the extractable attribute to true, the service creates a standard key that you can store in your apps or services.|
{: caption="Table 1. Describes the variables that are needed to add a standard key." caption-side="bottom"}

To protect the confidentiality of your personal data, avoid entering personally identifiable information (PII), such as your name or location, when you add keys to the service.
{: important}

A successful `POST api/v2/keys` response returns the ID value for your key, along with other metadata. The ID is a unique identifier that is assigned to your key and is used for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

**Optional**: Verify that the key was created by running the following call to get the keys in your {{site.data.keyword.keymanagementserviceshort}} service instance.

```sh
$ curl -X GET \
    "https://<regon>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

## What's next
{: #create-standard-key-next-steps}

- To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
