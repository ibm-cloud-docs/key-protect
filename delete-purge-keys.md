---

copyright:
  years: 2024
lastupdated: "2024-02-23"

keywords: key purge, automatic purge, manual purge

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

# About deleting and purging keys
{: #delete-purge-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to delete an encryption key and its key material if you are a manager for your {{site.data.keyword.keymanagementserviceshort}} instance.
{: shortdesc}

Before you can delete an instance, you must delete every key in that instance. However, if you close your account, any existing instances and keys are automatically hard deleted. Check out [Account cancelation and data deletion](/docs/key-protect?topic=key-protect-security-and-compliance#account-cancelation) for more information.
{: important}

In the event that a key is no longer needed or should be removed, {{site.data.keyword.keymanagementserviceshort}} allows you to delete and ultimately purge keys, an action that shreds the key material and makes any of the data encrypted with it inaccessible.

Deleting a key moves it into a _Destroyed_ state, a "soft" deletion in which the key can still be seen and restored for 30 days. After 90 days, the key will be automatically purged, or "hard deleted", and its associated data will be permanently shredded and removed from the {{site.data.keyword.keymanagementserviceshort}} service. If it is desirable that a key be purged sooner than 90 days, it is also possible to hard delete a key four hours after it has been moved into the _Destroyed_ state.

After a key has been deleted, any data that is encrypted by the key becomes inaccessible, though this can be reversed if the key is restored within the 30-day time frame. After 30 days, key metadata, registrations, and policies are available for up to 90 days, at which point the key becomes eligible to be purged. Note that once a key is no longer restorable and has been purged, its associated data can no longer be accessed. As a result, [destroying resources](/docs/key-protect?topic=key-protect-security-and-compliance#data-deletion) is not recommended for production environments unless absolutely necessary.

The following table lists the time frames in which you can view, restore, and purge a key after it has been deleted.

| Time from key deletion  | Name of key state | Can view/access key data? | Can restore? | Can user initiate purge? |
|-------------------------|-------------------|---------------------------|--------------|--------------------------|
| One second - Four hours | Destroyed         | Yes                       | Yes          | No                       |
| 4 hours - 30 days       | Destroyed         | Yes                       | Yes          | Yes                      |
| 30-90 days              | Destroyed         | Yes                       | No           | Yes                      |
| After 90 days           | Purged `*`        | No                        | No           | Yes                      |
{: caption="Table 1. Lists how users can interact with keys during certain time intervals after a key has been deleted" caption-side="top"}

`*` Note: because purged keys are completely inaccessible and "destroyed" in the common usage of the word, there is technically no "purged" key state. Purged keys no longer exist and therefore don't have a "state" one way or another. However, it can be useful to think of "purged" as being a state as nonexistence is part of the lifecycle of a key.

Once a key has been purged, any API calls that use the Key ID of a purged key will result in a 404 HTTP Not Found error. If you are required to retain any data related to a purged key (key metadata, registrations, policies, etc) for an extended period of time, it is recommended to perform the necessary API or CLI calls to retrieve and store that data in your own storage device.
{: tip}

## Considerations before deleting and purging a key
{: #delete-purge-keys-considerations}

{{site.data.keyword.keymanagementserviceshort}} blocks the deletion of any key that's actively protecting any registered cloud resources. Before you delete a key:

1. [Review the registered IBM resources](/docs/key-protect?topic=key-protect-view-protected-resources) that are associated with the key. If needed, you can [force deletion on a key](/docs/key-protect?topic=key-protect-delete-keys&interface=api#delete-keys-force-delete) that's protecting a registered cloud resource. However, the action won't succeed if the key's associated resource is non-erasable due to a [retention policy](/docs/cloud-object-storage?topic=cloud-object-storage-immutable#immutable-terminology-policy), which is a Write Once Read Many (WORM) policy set on the customer's relevant cloud resource.
2. Verify whether a key has a retention policy by checking the `preventKeyDeletion` field of the [registration details](/docs/key-protect?topic=key-protect-view-protected-resources#view-protected-resources-api) for the key. Then, you must contact an account owner to remove the retention policy on each resource that is associated with the key before you can delete the key.
3. Verify the key's deletion authorization policy. By default, keys in {{site.data.keyword.keymanagementserviceshort}} only require a single deletion authorization by a user with the _Manager_ role However, if a [dual authorization policy has been set](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy), two users with the _Manager_ role will have to approve the deletion.

{{site.data.keyword.keymanagementserviceshort}} restricts the ability to purge keys after only four hours to users with the [_KeyPurge_ role](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-keys-specific-functions), which must be specifically set for a user as it is not enabled by default, even for the instance owner. This ability is restricted precisely because purged keys cannot be restored. If there is any doubt whether a key should be purged four hours after it has been deleted, **do not purge it**.
{: important}

## API Example
{: #delete-purge-keys-api-example}

Please refer to the prerequisites and configuration settings associated with the [API reference](/apidocs/key-protect#purgekey), before using this example.

```sh
curl -X DELETE
    https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/purge
    -H 'accept: application/vnd.ibm.kms.key+json'
    -H 'authorization: Bearer <IAM_token>'
    -H 'bluemix-instance: <instance_ID>'

```
{: codeblock}

Replace the variables in the example request according to the following table.

| Variable | Description |
| --- | --- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The unique identifier or alias for the key that you would like to purge.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 2. Describes the variables that are needed to purge a key with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="bottom"}

For a detailed description of the request, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](/apidocs/key-protect){: external}.
{: tip}

## What's next
{: #delete-purge-keys-whats-next}

To learn how to delete and purge a key using the UI, check out [Deleting keys using a single authorization](/docs/key-protect?topic=key-protect-delete-keys). For information about how to do this using the API, click the **API** tab at the beginning of the topic.

To learn how to delete and purge a key that holds a dual-authorization deletion policy using the UI, check out [Deleting keys using dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys). For information about how to do this using the API, click the **API** tab at the beginning of the topic.