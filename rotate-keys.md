---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-28"

keywords: rotate encryption key, encryption key rotation, rotate key API examples

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

# Manually rotating keys
{: #rotate-keys}

You can rotate your root keys manually by using
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

When you rotate your root key, you replace the key with new key material. This
process creates a new key version that you can use for
[rewrapping or reencrypting your data](/docs/key-protect?topic=key-protect-rewrap-keys).

To learn how key rotation helps you meet industry standards and cryptographic
best practices, see
[Rotating your encryption keys](/docs/key-protect?topic=key-protect-key-rotation).

Rotation is available only for root keys. To learn more about your key rotation
options in {{site.data.keyword.keymanagementserviceshort}}, check out
[Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Rotating root keys in the console
{: #rotate-key-gui}
{: ui}

[After you create a root key](/docs/key-protect?topic=key-protect-create-root-keys), complete the following steps to rotate the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in your service. If you have many keys, you can narrow your search by using the search bars to only search for enabled keys (since other kinds of keys cannot be rotated), keys in a particular key ring, and keys with a particular alias.

5. Once the key has been found, click the ⋯ icon to open a list of options for the key that you want to rotate.

6. From the options menu, click **Rotate** to open the **Rotation** side panel.

7. Under **Manage rotation policy**, select the 30-day intervals for key rotation you want. If a key is set to be rotated every `2` months, for example, it will be rotated every 60 days, regardless of the number of days in a particular month.

8. Click **Set policy** to establish this policy. If you want to rotate the key immediately, click **Rotate key**. Note: these actions are not mutually exclusive.

**For imported root keys only**, you must add base64 encoded key material that you want to store and manage in the service. Ensure that the key material is in 128, 192, or 256 bits and that the bytes of data (for example, 32 bytes for 256 bits) are encoded by using base64 encoding.
{: important}

## Rotating root keys with the API
{: #rotate-key-api}
{: api}

You can rotate a root key by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rotate
```
{: codeblock}

1. [Retrieve authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Copy the ID of the root key that you want to rotate.

    You can find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

3. Replace the key with new key material by running the following `curl`
   command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rotate" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json" \
        -d '{
                "payload": "<key_material>"
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br> For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br> Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|key_material|**Optional**The new base64 encoded key material that you want to store and manage in the service. This value is required if you initially imported the key material when you added the key to the service.<br><br>To rotate a key that was initially generated by {{site.data.keyword.keymanagementserviceshort}}, omit the payload attribute and pass an empty request entity-body. To rotate an imported key, provide a key material that meets the following requirements:<br><br>The key must be 128, 192, or 256 bits. The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.|
{: caption="Table 2. Describes the variables that are needed to rotate a specified key in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

A successful rotation request returns an HTTP `204 No Content` response,
which indicates that your root key was replaced by new key material.

### Optional: Verify key rotation
{: #rotate-key-api-verify}
{: api}

You can verify that a key has been rotated by issuing a list keys request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.

Review the `lastRotateDate` and `keyVersion` values in the response
entity-body to inspect the date and time that your key was last rotated.

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
            "state": 1,
            "extractable": false,
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "imported": false,
            "creationDate": "2020-03-12T03:50:12Z",
            "createdBy": "...",
            "algorithmType": "AES",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "CBC_PAD"
            },
            "algorithmBitSize": 256,
            "algorithmMode": "CBC_PAD",
            "lastUpdateDate": "2020-03-12T03:50:12Z",
            "lastRotateDate": "2020-03-12T03:49:01Z",
            "keyVersion": {
                "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
                "creationDate": "2020-03-12T03:50:12Z"
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

The `keyVersion` attribute contains identifying information that describes
the latest version of the root key.

You can also list the versions that are available for the key by using the
{{site.data.keyword.keymanagementserviceshort}} API. To learn more, see
[Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
{: tip}

### Using an import token to rotate a key
{: #rotate-keys-secure-api}
{: api}

If you initially imported a root key by using an import token, you can rotate
the key by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rotate
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To rotate a key, you must be assigned a _Writer_ or _Manager_ access policy
    for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the ID of the key that you want to rotate.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    request, or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. [Create and retrieve an import token](/docs/key-protect?topic=key-protect-create-import-tokens).

4. Use the import token to encrypt the key material that you want to use to
   rotate the existing key.

    To learn how to use an import token, check out
    [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).

5. Replace the existing key with new key material by running the following
   `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rotate" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -d '{
                "type": "application/vnd.ibm.kms.key+json",
                "name": "<key_alias>",
                "description": "<key_description>",
                "extractable": <key_type>,
                "payload": "<encrypted_key>",
                "encryptionAlgorithm": "RSAES_OAEP_SHA_256",
                "encryptedNonce": "<encrypted_nonce>",
                "iv": "<iv>"
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance residesS.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_alias|**Required**. A unique, human-readable name for easy identification of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|key_description|**Optional**.An extended description of your key. To protect your privacy, do not store your personal data as metadata for your key.|
|encrypted_key|**Required**. The encrypted key material that you want to store and manage in the service. The value must be base64 encoded. Ensure that the key material meets the following requirements:<br><br>The key must be 128, 192, or 256 bits.The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.|
|key_type|**Optional**A boolean value that determines whether the key material can leave the service.<br><br>When you set the extractable attribute to false, the service designates the key as a root key that you can use for wrap or unwrap operations.|
|encrypted_nonce|**Required**. The AES-GCM encrypted nonce that ensures that the bits you send as part of a request are exactly the same as what we receive. The nonce validates the key that you are restoring.<br><br>To learn more, see [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce).|
|iv|**Required**. The initialization vector (IV) that is generated by the AES-GCM algorithm when you encrypt a nonce. This value is used to decode the key for storage in the {{site.data.keyword.keymanagementserviceshort}} system.<br><br>To learn more, see [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce).|
{: caption="Table 3. Describes the variables that are needed to rotate a key that has an import token." caption-side="top"}

A successful rotation request returns an HTTP `204 No Content` response,
which indicates that your root key was replaced by new key material.

### Optional: Verify import token key rotation
{: #import-token-key-rotate-api-verify}
{: api}

You can verify that a key that was imported via import token has been rotated by issuing a get key metadata request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<key_id>` is the ID of the key, the `<instance_ID>` is the name of your instance, and your `<IAM_token>` is your IAM token.

Review the `lastRotateDate` and `keyVersion` values in the response
entity-body to inspect the date and time that your key was last rotated.

You can also list the versions that are available for the key by using the
{{site.data.keyword.keymanagementserviceshort}} API. To learn more, see
[Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
{: tip}
