---

copyright:
  years: 2017, 2026
lastupdated: "2026-07-16"

keywords: unable to view keys, list keys, empty key list, key permissions

subcollection: key-protect

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I unable to view or list keys?
{: #unable-to-list-keys-api}
{: troubleshoot}

When you try to list keys by using the
{{site.data.keyword.keymanagementserviceshort}} API, you're unable to view any
keys in a {{site.data.keyword.keymanagementserviceshort}} instance that you have
access to.

You call `GET api/v2/keys` to list the keys that are available in your service
instance. The system returns a response similar to the following JSON object:
{: tsSymptoms}

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 0
    }
}
```
{: screen}

You do not have the correct authorization to view the requested range of keys.
{: tsCauses}

Contact an administrator to check your permissions. If the
{{site.data.keyword.keymanagementserviceshort}} instance contains keys that
you're unable to view, verify that you're assigned the applicable
[level of access to keys](/docs/key-protect?topic=key-protect-grant-access-keys)
in the {{site.data.keyword.keymanagementserviceshort}} instance. If the instance
contains more than 200 keys, you need to use the
[`offset` and `limit` parameters](/docs/key-protect?topic=key-protect-view-keys#retrieve-subset-keys-api)
to list another subset of keys.
{: tsResolve}

For example, if you want to list keys 201 - 210 that are available in a service
instance, you use `../keys?offset=200&limit=10` to skip the first 200 keys.
