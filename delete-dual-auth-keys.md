---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-28"

keywords: delete keys with dual authorization, dual authorization, policy-based, key deletion, purge key, purge

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
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Deleting keys that have a dual-authorization deletion policy
{: #delete-dual-auth-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to safely delete encryption keys by using a dual authorization process.
{: shortdesc}

Before deleting keys, make sure to review the [Considerations before deleting and purging a key](/docs/key-protect?topic=key-protect-delete-purge-keys#delete-purge-keys-considerations).

## Considerations for a key that has a dual-authorization deletion policy
{: #delete-dual-auth-keys-api}

[Dual authorization policies](/docs/key-protect?topic=key-protect-manage-dual-auth) are set to ensure that keys are not deleted in error by requiring the authorization of two specified users. Whatever the reason for a dual authorization policy, the method for deletion is the same. One of the users authorized to delete the key schedules it for deletion, which must be confirmed by another user.

Before you delete a key by using dual authorization:

- **Determine who can authorize deletion of your {{site.data.keyword.keymanagementserviceshort}} resources.** To use dual authorization, be sure to identify a user who can set the key for deletion and another who can delete the key. Users with a _Writer_ or _Manager_ role can set keys for deletion, but only users with a _Manager_ role can delete keys.
- **Plan to delete the key within a seven day authorization period.** When the first user authorizes a key for deletion, it remains in the [_Active_ state](/docs/key-protect?topic=key-protect-key-states) for seven days, during which all key operations are allowed on the key. To complete the deletion, another user with a _Manager_ role can use the {{site.data.keyword.keymanagementserviceshort}} GUI or API to delete the key at any point during those seven days, at which time the key will be moved to the *Destroyed* state. Note that because it is impossible to purge an active key, another user must delete the key before it can purged.
- **The key and its associated data will be inaccessible 90 days after being deleted.** When you delete a key, it is "soft deleted", meaning that the key and its associated data will be restorable up to 30 days after deletion. You are able to still retrieve associated data such as key metadata, registrations, and policies for up to 90 days. After 90 days, the key becomes eligible to be automatically [purged](#delete-dual-auth-keys-key-purge), or hard deleted, and its associated data will be permanently removed from the {{site.data.keyword.keymanagementserviceshort}} service.

## Authorize deletion for a key in the console
{: #delete-dual-auth-keys-set-key-deletion-console}
{: ui}

[After you enable dual authorization for an instance or key](/docs/key-protect?topic=key-protect-manage-dual-auth), you can provide the first authorization to delete a key by using the {{site.data.keyword.keymanagementserviceshort}} {{site.data.keyword.cloud_notm}} console.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in
   your service.

5. Click the ⋯ icon to open a list of options for the key that you want to delete.

6. From the options menu, click **Schedule key deletion** and review the key's
   associated resources.

7. Click the `Next` button, enter the key name, and click `Schedule deletion`.

8. Contact another user to complete the deletion of the key.

The other user must have _Manager_ access policy for the instance or key in order to authorize the key for deletion.
{: note}



## Authorize deletion for a key with the API
{: #delete-dual-auth-keys-set-key-deletion-api}
{: api}

[After you enable dual authorization for an instance or key](/docs/key-protect?topic=key-protect-manage-dual-auth),
you can provide the first authorization to delete a key by making a `POST` call
to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/setKeyForDeletion
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To set a key for deletion, you must be assigned a _Manager_ or _Writer_
    access policy for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Copy the ID of the key that you want to set or authorize for deletion.

3. Provide the first authorization to delete the key.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/setKeyForDeletion" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}}instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 1. Describes the variables that are needed to set a key for deletion." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that your key was authorized for deletion. Another user with a
_Manager_ access policy can now
[delete the key](/docs/key-protect?topic=key-protect-delete-keys)
by using the {{site.data.keyword.keymanagementserviceshort}} GUI or API.

If you need to prevent the deletion of a key that's already authorized for
deletion, you can remove the existing authorization by calling
`POST /api/v2/keys/<key_ID>/actions/unsetKeyForDeletion`.
{: tip}

### Delete the key
{: #delete-dual-auth-keys-key-api}
{: api}

After you set a key for deletion, another user with a _Manager_ access policy
can safely delete the key by using the
{{site.data.keyword.keymanagementserviceshort}} GUI or API.

{{site.data.keyword.keymanagementserviceshort}} sets a seven-day authorization period that
starts after you provide the first authorization to delete the key. During this
seven-day period, the key remains in the
[_Active_ state](/docs/key-protect?topic=key-protect-key-states)
and all key operations are allowed on the key. If no action is taken by another user and the seven-day period expires, you must
[restart the dual authorization process](#delete-dual-auth-keys-set-key-deletion-api)
 to delete the key.
{: note}

Delete a key and its contents by making a `DELETE` call to the following
endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you would like to delete.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    request, or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to delete the key and its contents.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>" \
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
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|return_preference|**Optional**.A header that alters server behavior for POST and DELETE operations.<br><br>When you set the return_preference variable to return=minimal, the service returns a successful deletion response. When you set the variable to return=representation, the service returns both the key material and the key metadata.|
{: caption="Table 2. Describes the variables that are needed to delete a key." caption-side="top"}

If the `return_preference` variable is set to `return=representation`, the
details of the `DELETE` request are returned in the response entity-body.

After you delete a key, it transitions to the `Deactivated` key
state. After 24 hours, if a key is not reinstated, the key
transitions to the `Destroyed` state. Destroyed keys can be
recovered after up to 30 days or their expiration date, whichever
is sooner. After this the key contents are permanently erased
and no longer accessible.

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

### Key purge
{: #delete-dual-auth-keys-key-purge}

When you delete a key, you immediately deactivate its key material and move it to a backstore in the {{site.data.keyword.keymanagementserviceshort}} service. Four hours after a key is deleted, the key becomes available to be manually purged. Thirty days after a key is deleted, the key becomes non-restorable and the key material is destroyed. After a key has been deleted for 90 days, if not manually purged, the key becomes eligible to be automatically purged and all its associated data will be permanently removed, or "hard deleted", from the {{site.data.keyword.keymanagementserviceshort}} service.

For more information about deleting and purging keys, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).

The following table lists which APIs you can use to retrieve data related to a deleted key.

| API               | Description                                                   |                                                          |
|-----------------------------------------------------------------------------------|----------------------------------------------------------|
| [Get key](/docs/key-protect?topic=key-protect-retrieve-key)                       | Retrieve key details                                     |
| [Get key metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata)     | Retrieve key metadata                                    |
| [Get registrations](/docs/key-protect?topic=key-protect-view-protected-resources) | Retrieve a list of registrations associated with the key |
{: caption="Table 4. Lists the API that users can use to view details about a key and its registrations." caption-side="top"}

### Removing an existing authorization
{: #delete-dual-auth-keys-unset-key-deletion-api}
{: api}

If you need to cancel an authorization for a key before the seven-day authorization period
expires, you can remove the existing authorization by making a `POST` call to
the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/unsetKeyForDeletion
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To remove an authorization to delete a key, you must be assigned a _Manager_
    or _Writer_ access policy for the instance or key. To learn how IAM roles
    map to {{site.data.keyword.keymanagementserviceshort}} service actions,
    check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Copy the ID of the key that you want to unset or deauthorize for deletion.

3. Remove an existing authorization to delete the key.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/unsetKeyForDeletion" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south`or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 5. Describes the variables that are needed to unset a key for deletion." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that your key is no longer authorized for deletion. If you need to
restart the dual authorization process, you can issue another authorization
to
[set the key for deletion](#delete-dual-auth-keys-set-key-deletion-api).
