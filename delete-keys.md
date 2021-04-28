---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-28"

keywords: delete key, delete key API examples, purge key

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

# Deleting keys using a single authorization
{: #delete-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to delete an encryption key and its key material, if you are a manager for your {{site.data.keyword.keymanagementserviceshort}} instance.
{: shortdesc}

Before deleting keys, make sure to review the [Considerations before deleting and purging a key](/docs/key-protect?topic=key-protect-delete-purge-keys#delete-purge-keys-considerations).

## Deleting keys in the console
{: #delete-keys-gui}
{: ui}

By default, {{site.data.keyword.keymanagementserviceshort}} requires one
authorization to delete a key. If you prefer to delete your encryption keys by
using a graphical interface, you can use the {{site.data.keyword.cloud_notm}}
console.

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys),
complete the following steps to delete a key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in
   your service.

5. Click the ⋯ icon to open a list of options for the key that you want to
   delete.

6. From the options menu, click **Delete key** and confirm the key deletion in the next screen by ensuring the key has no associated resources.

After you delete a key, the key transitions to the _Destroyed_
state. The data encrypted by keys in this state are no longer
accessible. Metadata that is associated with the key, such as the
key's deletion date, is kept in the
{{site.data.keyword.keymanagementserviceshort}} database. Destroyed
keys can be recovered after up to 30 days or their expiration date,
whichever is sooner.

## Deleting keys with the API
{: #delete-keys-api}
{: api}

By default, {{site.data.keyword.keymanagementserviceshort}} requires one
authorization to delete a key. You can delete a key and its contents by making a
`DELETE` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```

This action won't succeed if the key is actively protecting one or more cloud
resources. You can
[review the resources](/docs/key-protect?topic=key-protect-view-protected-resources)
that are associated with the key, or
[use the `force` parameter](#delete-key-force)
at query time to delete the key.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you want to delete.

   You can find the ID for a key in your
   {{site.data.keyword.keymanagementserviceshort}} instance by
   [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
   or by accessing the {{site.data.keyword.keymanagementserviceshort}}
   dashboard.

3. Run the following `curl` command to delete the key and its contents.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "prefer: <return_preference>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the key that you would like to delete.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of.<br><br>If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|return_preference|**Optional**.A header that alters server behavior for POST and DELETE operations.<br><br>When you set the return_preference variable to return=minimal, the service returns a successful deletion response. When you set the variable to return=representation, the service returns both the key material and the key metadata.|
{: caption="Table 1.  Describes the variables that are needed to delete keys with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}


If the `return_preference` variable is set to `return=representation`, the
details of the `DELETE` request are returned in the response entity-body.

After you delete a key, it transitions to the `Deactivated` key
state. After 24 hours, if a key is not reinstated, the key
transitions to the `Destroyed` state. Destroyed keys are recoverable
for up to 30 days or until their expiration date, after which
the key contents are permanently erased and no longer accessible.

The following JSON object shows an example returned value.

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
            "state": 5,
            "extractable": false,
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "imported": false,
            "creationDate": "2020-03-10T20:41:27Z",
            "createdBy": "...",
            "algorithmType": "AES",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "CBC_PAD"
            },
            "algorithmBitSize": 256,
            "algorithmMode": "CBC_PAD",
            "lastUpdateDate": "2020-03-16T20:41:27Z",
            "dualAuthDelete": {
                "enabled": false
            },
            "deleted": true,
            "deletionDate": "2020-03-16T21:46:53Z",
            "deletedBy": "..."
        }
    ]
}
```
{: screen}

For a detailed description of the available parameters, see the
{{site.data.keyword.keymanagementserviceshort}}
[REST API reference doc](/apidocs/key-protect){: external}.

### Using the `force` query parameter
{: #delete-keys-force-delete}
{: api}

{{site.data.keyword.keymanagementserviceshort}} blocks the deletion of a key
that's protecting a cloud resource, such as a Cloud Object Storage bucket. You
can force delete a key and its contents by making a `DELETE` call to the
following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?force=true
```

When you delete a key that has registrations associated with it, you immediately deactivate its key material and the data encrypted by the key. Any data that is encrypted by the key becomes inaccessible. Thirty days after a key is deleted, the key can no longer be restored and the key material will be destroyed after 90 days.
{: note}

Force deletion on a key won't succeed if the key is protecting a registered IBM
Cloud resource that's non-erasable due to a
[retention policy](/docs/cloud-object-storage?topic=cloud-object-storage-immutable#immutable-terminology-policy),
which is a Write Once Read Many (WORM) policy set on the your IBM cloud resource. You can verify whether a key is
associated with a non-erasable resource by
checking the 'preventKeyDeletion" field in the [registration details](/docs/key-protect?topic=key-protect-view-protected-resources)
for the key. Then, you must contact an account owner to remove the retention
policy on each registered IBM Cloud resource that is associated with the key before you can delete
the key.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you want to force delete.

   You can retrieve the ID for a specified key by making a `GET /v2/keys/`
   request, or by viewing your keys in the
   {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to force delete the key and its contents.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?force=true" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "prefer: <return_preference>"
    ```
    {: codeblock}

   Replace the variables in the example request according to the following
   table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the key that you would like to delete.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see[Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|return_preference|**Optional**.A header that alters server behavior for POST and DELETE operations.<br><br>When you set the return_preference variable to return=minimal, the service returns a successful deletion response. When you set the variable to return=representation, the service returns both the key material and the key metadata.|
{: caption="Table 2. Describes the variables that are needed to delete keys with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}


If the `return_preference` variable is set to `return=representation`, the
details of the `DELETE` request are returned in the response entity-body.

After you delete a key, it transitions to the `Deactivated` key
state. After 24 hours, if a key is not reinstated, the key
transitions to the `Destroyed` state. Destroyed keys can be recovered
after up to 30 days or their expiration date, whichever is sooner.
After this the key contents are permanently erased and no longer
accessible.

The following JSON object shows an example returned value.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "type": "application/vnd.ibm.kms.key+json",
            "aliases": [
                "alias-1",
                "alias-2"
            ],
            "name": "test-root-key",
            "description": "...",
            "state": 5,
            "expirationDate": "2020-03-15T20:41:27Z",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:30372f20-d9f1-40b3-b486-a709e1932c9c:key:2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "deleted": true,
            "algorithmType": "AES",
            "createdBy": "...",
            "deletedBy": "...",
            "creationDate": "2020-03-10T20:41:27Z",
            "deletionDate": "2020-03-16T21:46:53Z",
            "lastUpdateDate": "2020-03-16T20:41:27Z",
            "extractable": true
        }
    ]
}
```
{: screen}

For a detailed description of the available parameters, see the
{{site.data.keyword.keymanagementserviceshort}}
[REST API reference doc](/apidocs/key-protect){: external}.

## Key purge
{: #delete-keys-key-purge}

When you delete a key, you immediately deactivate its key material and move it to a backstore in the {{site.data.keyword.keymanagementserviceshort}} service. Four hours after a key is deleted, the key becomes available to be manually purged. Thirty days after a key is deleted, the key becomes non-restorable and the key material is destroyed. After a key has been deleted for 90 days, if not manually purged, the key becomes eligible to be automatically purged and all its associated data will be permanently removed, or "hard deleted", from the {{site.data.keyword.keymanagementserviceshort}} service.

For more information about deleting and purging keys, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).

The following table lists which APIs you can use to retrieve data related to a deleted key.

| API               | Description                                                   |                                                          |
|-----------------------------------------------------------------------------------|----------------------------------------------------------|
| [Get key](/docs/key-protect?topic=key-protect-retrieve-key)                       | Retrieve key details                                     |
| [Get key metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata)     | Retrieve key metadata                                    |
| [Get registrations](/docs/key-protect?topic=key-protect-view-protected-resources) | Retrieve a list of registrations associated with the key |
{: caption="Table 4. Lists the API that users can use to view details about a key and its registrations." caption-side="top"}
