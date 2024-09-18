---

copyright:
  years: 2020, 2024
lastupdated: "2024-09-18"

keywords: instance settings, service settings, dual authorization

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
{:cli: .ph data-hd-interface='cli'}

# Using dual authorization for the deletion of keys
{: #manage-dual-auth}

After you set up your {{site.data.keyword.keymanagementservicelong}} service instance, you can manage dual authorization (also known as "dual auth") by using the console, the API, or the CLI. This guide shows the instructions for using the console and the API. For instructions about the CLI, check out the [{{site.data.keyword.keymanagementserviceshort}} CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference).
{: shortdesc}

## Managing dual authorization settings
{: #manage-dual-auth-instance-policy}

Dual authorization for {{site.data.keyword.keymanagementserviceshort}} is a policy that helps to prevent accidental or malicious deletion of keys by requiring two administrators to approve its deletion. This policy can be set at the instance level, where it is automatically applied to any subsequently created keys, or on specific keys. Whatever the method used to generate the policy on a key, the authorization of two administrators is required to delete the key.

Links to various sections (all the h3s):

Considerations

Setting and removing policy on instance (console and api)

Setting policy on keys (only api)

Deleting keys with dual auth

### Considerations for setting a dual auth policy
{: #manage-dual-auth-instance-policy-considerations}

- **When you enable dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance, the policy applies to all subsequent keys.**
    By enabling dual authorization at the instance level, any new keys that you add to the instance automatically inherit the dual authorization policy. This is helpful in scenarios when a user with a _Manager_ level of permission on an instance wants to set a policy that applies to all subsequently created keys, regardless of which user creates the key. Because the instance level policy can only be changed by a user with a _Manager_ level permission (or a specifically tailored role that includes the ability to set dual auth policies), the _Manager_ can be assured that any subsequently created keys have the policy. Note that your existing keys are not affected by the policy change and will still require a single authorization for deletion.

- **You can always disable a dual authorization policy for your {{site.data.keyword.keymanagementserviceshort}} instance.**
    If you want to [disable an existing dual authorization policy](#disable-dual-auth-instance-policy-ui) to allow for single authorization, keep in mind that the change is applicable only for future keys that you add to the instance. Any existing keys that were created under a dual authorization policy will continue to require actions from two users before the keys can be deleted. After a key inherits a dual authorization policy, the policy cannot be reverted.

- **Once a dual auth policy has been applied to a key, the policy cannot be changed.**
    While it is possible to enable or disable a dual auth policy on an instance, a dual auth policy on a key cannot be disabled. The key retains the policy until it is deleted.

- **You can only apply a dual auth policy to a specific key using the API.** 
    A dual auth policy for an individual key cannot be set in the console. You must use either the CLI or the API. Similarly, whether a key has a dual auth policy cannot be seen in the console.

- **Do not set dual auth unless you have a second account admin. Otherwise the key cannot be deleted.**
    By definition, dual auth policies require two users to delete a key that holds the policy. As a result, the best practice is to ensure that you have a second user before establishing the policy on an instance or key.

- **To work with dual authorization policies, you must be assigned a _Manager_ access policy for the instance or key.** 
    To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

For information about deleting a key with a dual auth policy, check out [](#delete-dual-auth-keys-api)

### Enabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #enable-dual-auth-instance-policy-ui}
{: ui}

If you prefer to disable a dual authorization policy on your instance by using a graphical interface, you can use the console.

After creating a {{site.data.keyword.keymanagementserviceshort}} instance,
complete the following steps to create a dual authorization policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click the **Instance policies** link on the left side of the page.

    - Find the `Dual authorization delete` panel (on the top-left side of the
        page).

    - Toggle `Dual authorization deletion` to enable or disable the policy.

    - Click `Save` or `Cancel` (whichever is appropriate).

### Enabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #enable-dual-auth-instance-policy-api}
{: api}

As an instance manager, enable a dual authorization policy for a {{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a _Manager_ access policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable a dual authorization policy for your {{site.data.keyword.keymanagementserviceshort}} instance by running the following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "policy_type": "dualAuthDelete",
                        "policy_data": {
                            "enabled": true
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of.<br> If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 1. Describes the variables that are needed to enable dual authorization at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which indicates that your {{site.data.keyword.keymanagementserviceshort}} instance is now enabled for dual authorization. Keys that you create or import to the service now require two authorizations before they can be deleted. For more information, see
[Deleting keys](#delete-dual-auth-keys).

#### Optional: Verify dual auth policy enablement
{: #dual-auth-api-verify}
{: api}

You can verify that a dual auth policy key has been enabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.

### Disabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the console
{: #disable-dual-auth-instance-policy-ui}
{: ui}

If you prefer to disable a dual authorization policy on your instance by using a graphical interface, you can use the console.

After creating a {{site.data.keyword.keymanagementserviceshort}} instance, complete the following steps to create a dual authorization policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Instance policies** page, use the **Policies** table to browse the policies in your {{site.data.keyword.keymanagementserviceshort}} instance.

5. Click the ⋯ icon to open a list of options for the policy that you want to disable.

6. From the options menu, click **Disable policy** and confirm the policy was disabled in the updated **Policies** table.

### Disabling dual authorization for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #disable-dual-auth-instance-policy-api}
{: api}

As an instance manager, you can disable an existing dual authorization policy for a {{site.data.keyword.keymanagementserviceshort}} instance by making a `PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable and disable dual authorization policies, you must be assigned a _Manager_ access policy for your {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Disable an existing dual authorization policy for your {{site.data.keyword.keymanagementserviceshort}} instance by running the following `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
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
                            "enabled": false
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of.<br>If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 1. Describes the variables that are needed to enable dual authorization at the instance level." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which indicates that the dual authorization policy was updated for your service instance. Keys that you create or import to the service now require only one authorization before they can be deleted. For more information, see [Deleting keys](/docs/key-protect?topic=key-protect-delete-keys).

#### Optional: Verify dual auth policy disablement
{: #dual-auth-disable-api-verify}
{: api}

You can verify that a dual auth policy key has been disabled by issuing a list policies request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=dualAuthDelete" \
    -H "accept: application/vnd.ibm.kms.policy+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<instance_ID>` is the name of your instance and your `<IAM_token>` is your IAM token.

#### Setting dual authorization policies on a specific key
{: #set-dual-auth-key-policy}
{: api}

You can also use {{site.data.keyword.keymanagementservicefull}} to set dual authorization policies for individual keys. Not that this action can only be achieved using the API or CLI.

After you enable dual authorization at the key level, the policy that is associated with the key can no longer be changed to allow a single authorization to delete the key.
{: important}

### Viewing a dual authorization policy for a key
{: #view-dual-auth-key-policy-api}
{: api}

For a high-level view, you can retrieve the dual authorization policy for a single key by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the dual authorization policy for a specified key by running the following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|keyID_or_alias|**Required**. The unique identifier or alias for the key that has an existing rotation policy.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM Token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 1. Describes the variables that are needed to view a dual authorization policy for a key with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful request returns dual authorization policy details that are associated with your key. The following JSON object shows an example response for a key that has an existing dual authorization policy.

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

For keys that do not have an existing dual authorization policy, the following JSON shows an example response.

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
{: api}

Create a dual authorization policy for single key by making a `PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/policies?policy=dualAuthDelete
```
{: codeblock}

After you enable a dual authorization policy for a single key, the policy cannot be reverted.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable dual authorization for a specified key by running the following `curl` command.

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

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|keyID_or_alias|**Required**. The unique identifier or alias for the key that you want to create a dual authorization policy for.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 2. Describes the variables that are needed to update a dual authorization policy with the  {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful request returns a `200 OK` response with dual authorization policy details for your key. The following JSON object shows an example response.

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

The key now requires an authorization from two users before it can be deleted.

### Deleting keys that have a dual auth policy
{: #delete-dual-auth-keys}

Deleting a key that has a dual auth policy can be achieved using the console, the API, or the CLI. Whatever the reason for a dual authorization policy, the method for deletion is the same. One of the users authorized to delete the key schedules it for deletion, which must be confirmed by another user.

You can use {{site.data.keyword.keymanagementservicefull}} to safely delete encryption keys by using a dual authorization process.

Before deleting keys, make sure to review the [Considerations before deleting and purging a key](/docs/key-protect?topic=key-protect-delete-purge-keys#delete-purge-keys-considerations).

#### Considerations for deleting a key that holds a dual auth policy
{: #delete-dual-auth-keys-api}

Before you delete a key by using dual authorization:

- **Determine who can authorize deletion of your {{site.data.keyword.keymanagementserviceshort}} resources.** 
    To use dual authorization, be sure to identify a user who can set the key for deletion and another who can delete the key. Users with a _Writer_ or _Manager_ role can set keys for deletion, but only users with a _Manager_ role can delete keys.

- **Plan to delete the key within a seven day authorization period.** 
    When the first user authorizes a key for deletion, it remains in the [_Active_ state](/docs/key-protect?topic=key-protect-key-states) for seven days, during which all key operations are allowed on the key. To complete the deletion, another user with a _Manager_ role can use the {{site.data.keyword.keymanagementserviceshort}} GUI or API to delete the key at any point during those seven days, at which time the key will be moved to the *Destroyed* state. Note that because it is impossible to purge an active key, another user must delete the key before it can purged.

- **The key and its associated data will be inaccessible 90 days after being deleted.** 
    When you delete a key, it is "soft deleted", meaning that the key and its associated data will be restorable up to 30 days after deletion. You are able to still retrieve associated data such as key metadata, registrations, and policies for up to 90 days. After 90 days, the key becomes eligible to be automatically [purged](#delete-dual-auth-keys-key-purge), or hard deleted, and its associated data will be permanently removed from the {{site.data.keyword.keymanagementserviceshort}} service.

#### Authorize deletion for a key in the console
{: #delete-dual-auth-keys-set-key-deletion-console}
{: ui}

[After you enable dual authorization for an instance or key](#manage-dual-auth), you can provide the first authorization to delete a key by using the {{site.data.keyword.keymanagementserviceshort}} {{site.data.keyword.cloud_notm}} console.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

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

##### Purging a key that holds dual authorization in the console
{: #delete-dual-auth-keys-set-key-deletion-console-purge}
{: ui}

Four hours after the other user with a _Manager_ access policy has authorized the key for deletion, it can be purged by one of the users as long as they hold [the _KeyPurge_ attribute](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-keys-specific-functions).

This can be done by clicking the ⋯ icon to open a list of options for the key that you want to purge and then clicking **Purge**. If you cannot delete the key, make sure it has been at least four hours since the key was authorized for deleting by another user and that you hold the _KeyPurge_ attribute.

#### Authorize deletion for a key with the API
{: #delete-dual-auth-keys-delete-dual-auth-keys-set-key-deletion-api}
{: api}

After you enable dual authorization for an instance or key, you can provide the first authorization to delete a key by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/setKeyForDeletion
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To set a key for deletion, you must be assigned a _Manager_ or _Writer_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Copy the ID of the key that you want to set or authorize for deletion.

3. Provide the first authorization to delete the key.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/setKeyForDeletion" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}}instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID_or_alias|**Required**. The unique identifier or alias for the root key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 1. Describes the variables that are needed to set a key for deletion." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which indicates that your key was authorized for deletion. Another user with a _Manager_ access policy can now [delete the key](/docs/key-protect?topic=key-protect-delete-keys) by using the {{site.data.keyword.keymanagementserviceshort}} console or API.

If you need to prevent the deletion of a key that's already authorized for deletion, you can remove the existing authorization by calling `POST /api/v2/keys/<keyID_or_alias>/actions/unsetKeyForDeletion`.
{: tip}

#### Delete the key
{: #delete-dual-auth-keys-key-api}
{: api}

After you set a key for deletion, another user with a _Manager_ access policy can safely delete the key by using the {{site.data.keyword.keymanagementserviceshort}} GUI or API.

{{site.data.keyword.keymanagementserviceshort}} sets a seven-day authorization period that starts after you provide the first authorization to delete the key. During this seven-day period, the key remains in the [_Active_ state](/docs/key-protect?topic=key-protect-key-states) and all key operations are allowed on the key. If no action is taken by another user and the seven-day period expires, you must restart the dual authorization process to delete the key.
{: note}

Delete a key and its contents by making a `DELETE` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you would like to delete.

    You can retrieve the ID for a specified key by making a `GET /v2/keys` request, or by viewing your keys in the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to delete the key and its contents.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>" \
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
|key_ID_or_alias|**Required**. The unique identifier or alias for the key that you would like to delete.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|return_preference|**Optional**. A header that alters server behavior for POST and DELETE operations.<br><br>When you set the return_preference variable to return=minimal, the service returns a successful deletion response. When you set the variable to return=representation, the service returns both the key material and the key metadata.|
{: caption="Table 2. Describes the variables that are needed to delete a key." caption-side="top"}

If the `return_preference` variable is set to `return=representation`, the details of the `DELETE` request are returned in the response entity-body.

After you delete a key, it transitions to the `Deactivated` key state. After 24 hours, if a key is not reinstated, the key transitions to the `Destroyed` state. Destroyed keys can be recovered after up to 30 days or their expiration date, whichever is sooner. After this the key contents are permanently erased and no longer accessible.

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
          "algorithmType": "Deprecated",
          "algorithmMetadata": {
              "bitLength": "256",
              "mode": "Deprecated"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "Deprecated",
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

For a detailed description of the available parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](/apidocs/key-protect){: external}.

#### Key purge
{: #delete-dual-auth-keys-key-purge}

When you delete a key, you immediately deactivate its key material and move it to a backstore in the {{site.data.keyword.keymanagementserviceshort}} service. Four hours after a key is deleted, the key becomes available to be manually purged. Thirty days after a key is deleted, the key becomes non-restorable and the key material is destroyed. After a key has been deleted for 90 days, if not manually purged, the key becomes eligible to be automatically purged and all its associated data will be permanently removed, or "hard deleted", from the {{site.data.keyword.keymanagementserviceshort}} service.

For more information about deleting and purging keys, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).

The following table lists which APIs you can use to retrieve data related to a deleted key.

| API                                                                               | Description                                              |
|-----------------------------------------------------------------------------------|----------------------------------------------------------|
| [Get key](/docs/key-protect?topic=key-protect-retrieve-key)                       | Retrieve key details                                     |
| [Get key metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata)     | Retrieve key metadata                                    |
| [Get registrations](/docs/key-protect?topic=key-protect-view-protected-resources) | Retrieve a list of registrations associated with the key |
{: caption="Table 4. Lists the API that users can use to view details about a key and its registrations." caption-side="top"}

#### Removing an existing authorization
{: #delete-dual-auth-keys-undelete-dual-auth-keys-set-key-deletion-api}
{: api}

If you need to cancel an authorization for a key before the seven-day authorization period expires, you can remove the existing authorization by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/unsetKeyForDeletion
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To remove an authorization to delete a key, you must be assigned a _Manager_ or _Writer_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Copy the ID of the key that you want to unset or deauthorize for deletion.

3. Remove an existing authorization to delete the key.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/unsetKeyForDeletion" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south`or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID_or_alias |**Required**. The unique identifier or alias for the root key that you want to rotate.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 5. Describes the variables that are needed to unset a key for deletion." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which indicates that your key is no longer authorized for deletion. If you need to restart the dual authorization process, you can issue another authorization to set the key for deletion.


