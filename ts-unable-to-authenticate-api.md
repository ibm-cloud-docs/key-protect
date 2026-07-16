---

copyright:
  years: 2017, 2026
lastupdated: "2026-07-16"

keywords: unable to authenticate, 401 unauthorized, API authentication, access token

subcollection: key-protect

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why am I unable to authenticate through the API?
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
