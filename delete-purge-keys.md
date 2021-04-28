---

copyright:
  years: 2021
lastupdated: "2021-04-28"

keywords: IBM, purge, automatic purge, manual purge, delete, destroy

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

In the event that a key is no longer needed or should be removed, {{site.data.keyword.keymanagementserviceshort}} allows you to delete and ultimately purge keys, an action that shreds the key material and makes any of the data encrypted with it inaccessible.

Deleting a key moves it into a _Destroyed_ state, a "soft" deletion in which the key can still be seen and restored for 30 days. After 90 days, the key will be automatically [purged](#delete-keys-key-purge), or "hard deleted", and its associated data will be permanently shredded and removed from the {{site.data.keyword.keymanagementserviceshort}} service. If it is desirable that a key be purged sooner than 90 days, it is also possible to hard delete a key four hours after it has been moved into the _Destroyed_ state.

After a key has been deleted, any data that is encrypted by the key becomes inaccessible, though this can be reversed if the key is restored within the 30-day time frame. After 30 days, key metadata, registrations, and policies are available for up to 90 days, at which point the key becomes eligible to be purged. Note that once a key is no longer restorable and has been purged, its associated data can no longer be accessed. As a result, [destroying resources](/docs/key-protect?topic=key-protect-security-and-compliance#data-deletion) is not recommended for production environments unless absolutely necessary.
{: important}

The following table lists the time frames in which you can view, restore, and purge a key after it has been deleted.

| Time from key deletion  | Name of key state | Can view/access key data? | Can restore? | Can user initiate purge? |
|-------------------------|-------------------|---------------------------|--------------|--------------------------|
| One second - Four hours | Destroyed         | Yes                       | Yes          | No                       |
| 4 hours - 30 days       | Destroyed         | Yes                       | Yes          | Yes                      |
| 30-90 days              | Destroyed         | Yes                       | No           | Yes                      |
| After 90 days           | Purged `*`        | No                        | No           | Yes                      |
{: caption="Table 3. Lists how users can interact with keys during certain time intervals after a key has been deleted" caption-side="top"}

`*` Note: because purged keys are completely inaccessible and "destroyed" in the common usage of the word, there is technically no "purged" key state. Purged keys no longer exist and therefore don't have a "state" one way or another. However, it can be useful to think of "purged" as being a state as nonexistence is part of the lifecycle of a key.

Once a key has been purged, any API calls that use the Key ID of a purged key will result in a 404 HTTP Not Found error. If you are required to retain any data related to a purged key (key metadata, registrations, policies, etc) for an extended period of time, it is recommended to perform the necessary API or CLI calls to retrieve and store that data in your own storage device.
{: tip}

## Considerations before deleting and purging a key
{: #delete-purge-keys-considerations}

{{site.data.keyword.keymanagementserviceshort}} blocks the deletion of any key that's actively protecting any registered cloud resources. Before you delete a key:

1. [Review the registered IBM resources](/docs/key-protect?topic=key-protect-view-protected-resources) that are associated with the key. If needed, you can [force deletion on a key](#delete-key-force) that's protecting a registered cloud resource. However, the action won't succeed if the key's associated resource is non-erasable due to a [retention policy](/docs/cloud-object-storage?topic=cloud-object-storage-immutable#immutable-terminology-policy), which is a Write Once Read Many (WORM) policy set on the customer's relevant cloud resource.
2. Verify whether a key has a retention policy by checking the `preventKeyDeletion` field of the [registration details](/docs/key-protect?topic=key-protect-view-protected-resources#view-protected-resources-api) for the key. Then, you must contact an account owner to remove the retention policy on each resource that is associated with the key before you can delete the key.
3. Verify the key's deletion authorization policy. By default, keys in {{site.data.keyword.keymanagementserviceshort}} only require a single deletion authorization by a user with the _Manager_ role However, if a [dual authorization policy has been set](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy), two users with the _Manager_ role will have to approve the deletion.

To learn how to delete keys that hold a single authorization policy, check out [Deleting keys using a single authorization](/docs/key-protect?topic=key-protect-delete-keys).

To learn how to delete keys that hold a dual authorization policy, check out [Deleting keys using dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys).
