---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-11"

keywords: failed to create key, failed to delete key, delete service

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:term: .term}

# Troubleshooting
{: #troubleshooting}

General problems with using {{site.data.keyword.keymanagementservicefull}} might
include providing the correct headers or credentials when you interact with the
API. In many cases, you can recover from these problems by following a few easy
steps.
{: shortdesc}

## Unable to create keys
{: #unable-to-create-keys}
{: troubleshoot}

When you access the {{site.data.keyword.keymanagementserviceshort}} user
interface, the option to add a key to the
{{site.data.keyword.keymanagementserviceshort}} instance is disabled.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of
the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You can see a list of keys, but you're unable to select the option to add a key.

You do not have the correct authorization to perform
{{site.data.keyword.keymanagementserviceshort}} actions.
{: tsCauses}

Verify with an administrator that you are assigned the correct role in the
applicable {{site.data.keyword.keymanagementserviceshort}} instance. For more
information about roles, see
[Roles and permissions](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
{: tsResolve}

## Unable to authenticate through the API
{: #unable-to-authenticate-api}
{: troubleshoot}

When you call the {{site.data.keyword.keymanagementserviceshort}} API, the
system returns a `401 Unauthorized` error, and you're unable to make the API
request.

You call any {{site.data.keyword.keymanagementserviceshort}} API method. You see
an error response similar to the following JSON object:
{: tsSymptoms}

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.error+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "errorMsg": "Unauthorized: The user does not have access to the specified resource"
        }
    ]
}
```
{: screen}

You do not have the correct authorization to perform
{{site.data.keyword.keymanagementserviceshort}} actions in the specified service
instance.
{: tsCauses}

Verify with an administrator that you are assigned the correct platform and
service access roles in the applicable
{{site.data.keyword.keymanagementserviceshort}} instance. For more information
about roles, see
[Roles and permissions](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
{: tsResolve}



## Unable to view or list keys
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

## Unable to view or list specific keys
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

The {{site.data.keyword.keymanagementserviceshort}} instance contains a large
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

## Unable to delete keys
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

## Getting help and support
{: #getting-help}

If you have problems or questions when you are using
{{site.data.keyword.keymanagementserviceshort}}, you can check
{{site.data.keyword.cloud_notm}}, or get help by searching for information or by
asking questions through a forum. You can also open a support ticket.
{: shortdesc}

You can check whether {{site.data.keyword.cloud_notm}} is available by going to
the
[status page](/status?tags=platform,runtimes,services){: external}.

You can review the forums to see whether other users ran into the same problem.
When you are using the forums to ask a question, tag your question so that it is
seen by the {{site.data.keyword.cloud_notm}} development teams.

- If you have technical questions about
    {{site.data.keyword.keymanagementserviceshort}}, post your question on
    [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external}
    and tag your question with "ibm-cloud" and "key-protect".

- For questions about the service and getting started instructions, use the
    [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external}
    forum. Include the "ibm-cloud" and "key-protect" tags.

See
[Asking a question](/docs/get-support?topic=get-support-using-avatar#asking-a-question){: external}
for more details about using the forums.

For more information about opening an {{site.data.keyword.IBM_notm}} support
ticket, or about support levels and ticket severities, see
[Contacting support](/docs/get-support){: external}.


