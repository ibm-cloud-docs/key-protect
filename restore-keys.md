---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-22"

keywords: restore key, restore a deleted key, re-import a key

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

# Restoring keys
{: #restore-keys}

When a key is deleted, it is moved to a "destroyed" state. However, information about the key (such as its metadata) can still be viewed and you have 30 days to restore the key to an active state. For this reason, a key deletion is considered a "soft delete" in that the key still exists but can no longer be used to access the data it has encrypted. This topic describes the process to restore a key and the limitations of the key restoration process.
{: shortdesc}

As part of a testing and development process, it is normal to regularly create and delete keys. These keys are not gone forever; rather they are "soft deleted" and moved to a **destroyed** state, meaning that the key cannot be used as part of any key actions such as encrypting or decrypting data. The key data can only be looked at, and, if the key was deleted in error, it might be possible to restore the key to an **active** state.

All keys, whether created in {{site.data.keyword.keymanagementserviceshort}} or imported, and whether a root or standard key, can be restored.
{: tip}

This intermediate period, in which a key has been deleted but can still be restored, lasts for 30 days. Between 30 days and 90 days, the key data can still be accessed, but the key can no longer be restored. After 90 days, the key becomes eligible to be automatically **purged**, which can occur at any time after 90 days. Purged keys, unlike destroyed keys, are gone forever.

| Time from key deletion | Name of key state | Can view/access key data? | Can restore? |
|------------------------|-------------------|---------------------------|--------------|
| One-30 days            | Destroyed         | Yes                       | Yes          |
| 30-90 days             | Destroyed         | Yes                       | No           |
| After 90 days          | Purged`*`         | No                        | No           |
{: caption="Table 1. Ties key states to the time from a key's deletion to what actions are possible with the key." caption-side="top"}

`*`Note: because purged keys are completely inaccessible and "destroyed" in the common usage of the word, there is technically no "purged" key state. Purged keys are simply gone and therefore don't have a "state" one way or another. However, it can be useful to think of "purged" as being a state because nonexistence is part of the lifecycle of a key. If you need to purge a key sooner than 90 days, it requires a special key purging role. For more information, check out [Deleting keys](/docs/key-protect?topic=key-protect-delete-keys).

For more information about key states, check out [Monitoring the lifecycle of encryption keys](/docs/key-protect?topic=key-protect-key-states).

Because {{site.data.keyword.keymanagementserviceshort}} does not allow instances that have keys in them to be deleted, it is required that any keys in an instance be deleted before the instance itself can be deleted. However, because of the "soft" deletion of keys, it is possible that a user might delete a key and then soon after delete an instance. This deletion of the instance also permanently deletes the key, even if the deletion of the key is recent enough to have made it eligible to be restored. For this reason, {{site.data.keyword.keymanagementserviceshort}} allows an instance to be reclaimed for a short time after it has been deleted. To see if your instance can be reclaimed, check out [Listing reclaimed resources by using the CLI](/docs/account?topic=account-resource-reclamation&interface=cli#list-reclaimed-cli). For the commands on reclaiming the resource (the instance in this case), check out [Restoring a resource by using the CLI](/docs/account?topic=account-resource-reclamation&interface=cli#restore-resource-cli).
{: important}

## How do I know whether a key can be restored?
{: #restore-keys-eligibility}
{: ui}

To see whether a destroyed key can be restored:

1. Navigate to your {{site.data.keyword.keymanagementserviceshort}} instance in the {{site.data.keyword.cloud_notm}} console.
2. In the left navigation, make sure you are on the **Keys** screen.
3. Find the key you want to restore. Note: only keys in a **Destroyed** state can be restored.
4. Under the `Last updated` column, note the date. Then refer to Table 1, which can be found above. If the key has been deleted within the last 30 days, it can be restored. If it has been more then 30 days, you will not be able to restore the key. If you attempt to restore a key that is no longer eligible to be restored, you will receive an error when trying to restore the key.

## Restoring a deleted key with the console
{: #restore-ui}
{: ui}

If you prefer to restore your key by using a graphical interface, you can use the {{site.data.keyword.cloud_notm}} console.

[After you import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys) and [delete](/docs/key-protect?topic=key-protect-delete-keys) a key, complete the following steps to restore the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** > **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click **Keys** to open the keys panel and find the key in the destroyed state you want to restore. One way to achieve this is to click the **Key states** drop-down list and select the **Destroyed** state. This will limit the results to only keys that have been deleted.

5. Click the ⋯ icon to open a list of options for the key that you want to restore. Note that if the key has just been moved to a _Destroyed_ state you must wait 30 seconds before attempting to restore it.

6. Click the **Restore Key** button to open the restore side panel.

7. Click **Restore Key** button.

8. Confirm the key was restored in the updated **Keys** table.

## Restoring a deleted key with the API
{: #restore-api}
{: api}

Restore a previously imported key by making a `POST` call to the following endpoint:

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/restore
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To restore a key, you must have the _Manager_ role for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

2. Retrieve the ID of the key that you want to restore.

    You can retrieve the ID for a specified key by making a [list keys request](/apidocs/key-protect#list-keys){: external} request, or by viewing your keys in the {{site.data.keyword.keymanagementserviceshort}} dashboard. For more information, check out [Viewing a list of keys](/docs/key-protect?topic=key-protect-view-keys).

3. Run the following `curl` command to restore the key and regain access to its associated data. Note that you must wait 30 seconds after deleting a key before you are able to restore it.

    You cannot restore a key that has an expiration date that is current or in the past.
    {: important}

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/restore" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to Table 2.

| Variable    | Description                                                                                               |
|-------------|-----------------------------------------------------------------------------------------------------------|
| region      | **Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, check out [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| keyID_or_alias      | **Required**. The unique identifier or alias for the key that you want to restore.                                 |
| IAM_token   | **Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the `IAM` token, including the `Bearer` value, in the `curl` request. For more information, check out [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token). |
| instance_ID | **Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, check out [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID). |
| key_ring_ID | **Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is therefore recommended to specify the key ring ID for a more optimized request.  Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: `default`.  For more information, check out [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys). |
{: caption="Table 2. Describes the variables that are needed to restore keys with the {{site.data.keyword.keymanagementserviceshort}} API" caption-side="top"}

    A successful restore request returns an HTTP `201 Created` response, which indicates that the key was restored to the _Active_ key state and is now available for encrypt and decrypt operations. All attributes and policies that were previously associated with the key are also restored.

    You will have access to data associated with the key as soon as the key is restored.
    {: note}

### Optional: Verify key restoration
{: #restore-api-verify}
{: api}

You can verify that the key has been restored by getting details about the key by issuing:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/metadata" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<keyID_or_alias>` is the ID or alias of the key, the `<instance_ID>` is the name of your instance, and your `<IAM_token>` is your IAM token.

Review the `state` field in the response body to verify that the key transitioned to the _Active_ key state. The following JSON output shows example metadata details for an _Active_ key.

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
            "name": "...",
            "description": "...",
            "tags": [
                "..."
            ],
            "state": 1,
            "extractable": false,
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "imported": true,
            "creationDate": "2020-03-10T20:41:27Z",
            "createdBy": "...",
            "algorithmType": "Deprecated",
            "algorithmMetadata": {
                "bitLength": "128",
                "mode": "Deprecated"
            },
            "algorithmBitSize": 128,
            "algorithmMode": "Deprecated",
            "lastUpdateDate": "2020-03-16T20:41:27Z",
            "keyVersion": {
                "id": "30372f20-d9f1-40b3-b486-a709e1932c9c",
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

The integer mapping for the _Active_ key state is `1`.
{: note}


