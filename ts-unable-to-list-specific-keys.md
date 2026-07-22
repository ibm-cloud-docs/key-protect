---

copyright:
  years: 2017, 2026
lastupdated: "2026-07-16"

keywords: unable to list specific keys, key not found, offset limit, key pagination

subcollection: key-protect

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I unable to view or list specific keys?
{: #unable-to-list-specific-keys}
{: troubleshoot}

When you call the {{site.data.keyword.keymanagementserviceshort}} API, you're
unable to list specific keys that you have access to.

You call `GET api/v2/keys` to list the keys that are available in your service
instance.
{: tsSymptoms}

You can see a list of keys, but you can't find a specific key that's stored in
the instance. You verify with your administrator that you're assigned the
applicable
[level of access to the keys](/docs/key-protect?topic=key-protect-grant-access-keys)
that you're unable to view. You also verify with your admin that the key belongs
to the {{site.data.keyword.keymanagementserviceshort}} instance that you're
targeting.

The {{site.data.keyword.keymanagementserviceshort}} instance contains a significant
number of keys, and the specific keys that you're looking for aren't returned by
default when you call `GET api/v2/keys` to list keys.
{: tsCauses}

Check with an admin to understand the total number of keys that are stored in
the instance. By default, `GET api/v2/keys` returns the first 200 keys. If the
{{site.data.keyword.keymanagementserviceshort}} instance contains more than 200
keys, you need to use the
[`offset` and `limit` parameters](/docs/key-protect?topic=key-protect-view-keys#retrieve-subset-keys-api)
to list another subset of keys.
{: tsResolve}

For example, if you want to list keys 201 - 210 that are available in a service
instance, you use `../keys?offset=200&limit=10` to skip the first 200 keys.
