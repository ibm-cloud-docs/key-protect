---

copyright:
  years: 2017, 2026
lastupdated: "2026-07-16"

keywords: unable to delete keys, 409 conflict, protected resource, force delete

subcollection: key-protect

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I unable to delete keys?
{: #unable-to-delete-keys}
{: troubleshoot}

When you use the {{site.data.keyword.keymanagementserviceshort}} user interface
or REST API, you're unable to delete a key.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of
the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You're assigned a _Manager_ access policy for the
{{site.data.keyword.keymanagementserviceshort}} instance. You try to delete a
key, but the action fails with the following error message.

```plaintext
Conflict: Key could not be deleted. Status: 409, Correlation ID: 160cc463-71d1-4b30-a5f2-d3f7e9f2b75e
```
{: screen}

You also try to delete the key by using the
{{site.data.keyword.keymanagementserviceshort}} API, but you receive the
following error message.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.error+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "errorMsg": "Conflict: Key could not be deleted. Please see `reasons` for more details.",
            "reasons": [
                {
                    "code": "PROTECTED_RESOURCE_ERR",
                    "message": "Key is protecting one or more cloud resources",
                    "status": 409,
                    "moreInfo": "https://cloud.ibm.com/apidocs/key-protect",
                    "target": {
                        "type": "query_param",
                        "name": "force"
                    }
                }
            ]
        }
    ]
}
```
{: screen}

This key is actively protecting one or more cloud resources, such as a {{site.data.keyword.cos_full}} bucket or a {{site.data.keyword.databases-for}} deployment.
{: tsCauses}

For your protection, {{site.data.keyword.keymanagementserviceshort}} prevents
the deletion of a key that's actively encrypting data in the cloud. Before you
delete a key,
[review which resources are encrypted by this key](/docs/key-protect?topic=key-protect-view-protected-resources)
and verify with the owner of the resources to ensure you no longer require
access to that data.
{: tsResolve}

You can get the current list of resources associated with your key by first [synchronizing the key](/docs/key-protect?topic=key-protect-sync-associated-resources), which might take up to 4 hours. Then, proceed to [viewing associations between root keys and encrypted {{site.data.keyword.cloud_notm}} resources](/docs/key-protect?topic=key-protect-view-protected-resources).

After using **`Sync`**, associations between the key and other resources will be current and up to date. If there are no associations after using **`Sync`**, the key can be deleted normally.

If the associations are still there after **`Sync`**:

- You can use the {{site.data.keyword.keymanagementserviceshort}} API to
    [force deletion on the key](/docs/key-protect?topic=key-protect-delete-keys#delete-keys-force-delete).

- You can delete the resources associated with the key, and then delete the key normally.
