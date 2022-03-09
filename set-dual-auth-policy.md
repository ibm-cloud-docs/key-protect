---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-09"

keywords: set deletion policy, dual authorization, policy-based, key deletion

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

# Setting dual authorization policies for keys
{: #set-dual-auth-key-policy}

You can use {{site.data.keyword.keymanagementservicefull}} to set dual
authorization policies for individual encryption keys.
{: shortdesc}

Dual authorization is an extra policy that helps to prevent accidental or
malicious deletion of keys in your
{{site.data.keyword.keymanagementserviceshort}} instance. When you
enable this policy at the key level,
{{site.data.keyword.keymanagementserviceshort}} requires an authorization from
two users to delete a key.

After you enable dual authorization at the key level, the policy that is
associated with the key can no longer be changed to allow a single authorization
to delete the key.
{: important}

To enable dual authorization settings at the instance level, check out
[Managing service settings](/docs/key-protect?topic=key-protect-manage-dual-auth).
{: tip}

## Managing dual authorization policies with the API
{: #manage-dual-auth-key-policies-api}

Consider the following items before you enable dual authorization for a key:

- **Determine whether a dual authorization policy is required for the key.**
    As a security admin, assess the sensitivity of your workload to determine
    whether a key requires a dual authorization policy based on your requirements.
    After you enable dual authorization for a key, the policy can no longer be
    changed to allow a single authorization to delete the key.

- **Determine who can authorize deletion of your {{site.data.keyword.keymanagementserviceshort}} resources.**
    After you create a dual authorization policy for a key, the key will require
    [an action from two users](/docs/key-protect?topic=key-protect-delete-dual-auth-keys)
    before it can be deleted. Be sure to identify two distinct users with the
    [appropriate levels of access](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles)
    to the instance or key.

To learn how to delete a key that has a dual authorization policy, see
[Deleting keys using dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys).

### Viewing a dual authorization policy for a key
{: #view-dual-auth-key-policy-api}

For a high-level view, you can retrieve the dual authorization policy for a
single key by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_
    access policy for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the dual authorization policy for a specified key by running the
    following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|keyID_or_alias|**Required**. The unique identifier or alias for the key that has an existing rotation policy.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM Token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 1. Describes the variables that are needed to view a dual authorization policy for a key with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful request returns dual authorization policy details that are
associated with your key. The following JSON object shows an example
response for a key that has an existing dual authorization policy.

```json
{
    "metadata": {
        "collectionTotal": 1,
        "collectionType": "application/vnd.ibm.kms.policy+json"
    },
    "resources": [
        {
            "id": "02fd6835-6001-4482-a892-13bd2085f75d",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "dualAuthDelete": {
                "enabled": true
            },
            "createdBy": "...",
            "creationDate": "2020-03-10T20:41:27Z",
            "updatedBy": "...",
            "lastUpdateDate": "2020-03-16T20:41:27Z"
        }
    ]
}
```
{: screen}

For keys that do not have an existing dual authorization policy, the
following JSON shows an example response.

```json
{
    "metadata": {
        "collectionTotal": 0,
        "collectionType": "application/vnd.ibm.kms.policy+json"
    }
}
```
{: screen}

### Creating a dual authorization policy for a key
{: #create-dual-auth-key-policy-api}

Create a dual authorization policy for single key by making a `PUT` call to the
following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete
```
{: codeblock}

After you enable a dual authorization policy for a single key, the policy cannot
be reverted.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_
    access policy for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable dual authorization for a specified key by running the following `curl`
    command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.policy+json",
                        "dualAuthDelete": {
                            "enabled": true
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|keyID_or_alias|**Required**. The unique identifier or alias for the key that you want to create a dual authorization policy for.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 2. Describes the variables that are needed to update a dual authorization policy with the  {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful request returns a `200 OK` response with dual authorization
policy details for your key. The following JSON object shows an example
response.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.policy+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:30372f20-d9f1-40b3-b486-a709e1932c9c:key:2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "dualAuthDelete": {
                "enabled": true
            },
            "createdBy": "...",
            "creationDate": "2020-03-10T20:41:27Z",
            "updatedBy": "...",
            "lastUpdateDate": "2020-03-16T20:41:27Z"
        }
    ]
}
```
{: screen}

The key now requires an authorization from two users before it can be
deleted.

For more information, see
[Deleting keys using dual authorization](/docs/key-protect?topic=key-protect-delete-dual-auth-keys).


