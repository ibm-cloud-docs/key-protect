---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-28"

keywords: user permissions, manage access, IAM roles, roles

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
{:term: .term}

# Understanding user roles and resources
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} supports a centralized access control system that leverages {{site.data.keyword.iamlong}} to help you assign your users the correct roles and access for your account, service instances, encryption keys, and key rings.
{: shortdesc}

Because {{site.data.keyword.keymanagementserviceshort}} is a key management system that, by its nature, involves encrypting important and often confidential data, it is vital that the permissions structure over the account, service instances, encryption keys, and key rings is both powerful and flexible. To this end, {{site.data.keyword.iamlong}} roles can be assigned in varying combinations, depending on the level of administration in question.

These varying kinds of access are analogous to many kinds of life situations. A person might be the founder and CEO of a company and yet only be a regular member of a local club, and likewise have no authority at all to write traffic tickets. In a similar way, {{site.data.keyword.keymanagementserviceshort}} roles are assigned within the context of a specific part of {{site.data.keyword.keymanagementserviceshort}}, although to make things simpler, {{site.data.keyword.keymanagementserviceshort}} does define "default" roles over certain resources unless specified otherwise (which we'll discuss in more detail later).

There are two main areas of administration for almost all {{site.data.keyword.cloud_notm}} products: the **account** (also known as the "platform"), and the **service instances** owned by the account. A large bank, for example, might have only one account (controlled by executive leadership) and separate service instances for each of the organizational units within the bank (for example, one unit might manage bank accounts while another manages loans). While it is likely that users with rights at the account level will also have rights over the various instances (and perhaps, though not always, the other way around), note that the names given to account roles are different than those for the roles within the service instances, reflecting this difference between account roles and service instance roles. For more information on those roles, their names, and their permissions, check out [IAM roles and actions](/docs/account?topic=account-iam-service-roles-actions).

## Best practices
{: #manage-access-best-practices}

* **Enable user access to the resources in your account by assigning {{site.data.keyword.iamshort}} roles.**
  Rather than the account owner sharing their credentials, which by default give them _Administrator_ and _Manager_ access over all resources under their account, create new policies for users who need access to the account and to each service instance (and its associated keys).

* **Grant roles and permissions at the smallest scope needed.**
  While it is certainly simpler to give all users _Manager_ or _Administrator_ roles over resources, this is not a good security practice. Instead, determine the minimum amount of permission a user needs in your use case and only grant them that level of permission. It is possible, for example, to only allow a user access to a single key, or to limit a user to only be a _Reader_ of a particular instance. For more information about assigning fine-grained access, check out [assign fine-grained access to a single key](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level).

* **Regularly audit who can manage access control and delete key resources.**
  Because the errors and misuse by users with elevated permissions can cause damage to your account, your service instances, and data protected by keys in your service instance, ensure that proper access is being maintained by auditing the users with those roles. Remember that any new keys, key rings, or service instances created are subject to existing role definitions by default. If a new key should not be accessed or modified by a new user being assigned as a _Manager_ of the instance, that restriction must be specifically assigned, as instance managers have access to all of the keys in an instance by default, including the ability to delete a key.

## Platform roles and service roles
{: #manage-access-roles}

The word "object" is used in this section as a broad term for things like keys or key rings or service instances or accounts.
{: important}

As mentioned earlier, roles exist at both the platform (account) and service level. If you are unsure about what a platform or a service role allows a user to do, remember that platform roles interact mainly with IBM Cloud services like the [resource controller](/docs/account?topic=account-overview) or {{site.data.keyword.iamshort}}. Roles inside of a service, on the other hand, interact mainly with the relevant API, which in this case is the {{site.data.keyword.keymanagementserviceshort}} API. This is why, as you'll see, platform roles have limited use inside of your service instances beyond (in the case of the _Administrator_ role) the ability to create an access policy for a particular object, such as a key ring.

**Platform roles**
  * Administrator  
    Has the full spectrum of rights over a particular object and its "child" objects (for example, keys are child objects of instances), including the right to invite new users and assign roles over the object (only administrators can assign roles). Note that administrators do not have service roles by default. They can, however, assign roles to themselves.
  * Editor  
    Can view, create, and delete instances at the account level, but cannot invite new users. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.
  * Operator  
    Can view instances at the account level, but cannot edit them. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.
  * Viewer  
    Can view instances at the account level, but cannot edit them. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.

Platform roles be assigned over an entire account, over particular service instances, or within objects inside of a service instance.

| Action | Viewer | Editor | Operator | Administrator |
| ------ | ------ | ------ | -------- | ------------- |
| View {{site.data.keyword.keymanagementserviceshort}} instances | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Create {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](..images/icons/checkmark-icon.svg) | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Delete {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](..images/icons/checkmark-icon.svg) | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Invite new users and manage access policies | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: caption="Table 1. Lists platform management roles as they apply to {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

While an account-level role gives a user particular permissions over service instances by default, roles can also be assigned over a particular service instance. For example, an account _Editor_ (who has the ability to view, create, and delete instances, but not the ability to assign roles) can be made an _Administrator_ of a particular service instance, allowing them to assign roles within that service instance.

Service roles can be applied to the three first class objects within a service instance: the **instance** as a whole, particular **keys**, and **key rings**. Just as account roles have permissions over instances by default, so too do instance managers have permissions over keys and key rings by default. However, these permissions can be assigned more granularly where necessary, for example giving a user the _Manager_ role over only a particular key or key ring and some lesser level of permission over the instance as a whole.

Service roles can be assigned per-instance or for all instances in an account.
{: tip}

**Service instance roles**
  * Manager  
    Has the full spectrum of rights over a particular object (for example, the manager of a key has the ability to wrap, unwrap, and delete the key, as well as the exclusive right to read and update {{site.data.keyword.keymanagementserviceshort}} policies such as `dualAuthDelete`, `allowedNetwork`, `allowedIP`, among others).
  * Writer  
    Has most of the same rights a manager does when it comes to using an object (including the ability to retrieve a key and its metadata), but generally cannot delete or disable the object.
  * Reader  
    Can use the object (for example, key readers can wrap and unwrap a key), but neither create, delete, or modify the object.
  * ReaderPlus  
    Have the same rights as a reader, with the additional ability to retrieve a standard key's payload.

These permissions are **additive**. An instance manager will always be the manager of all of the keys and key rings in the instance. Similarly, a key ring manager will always be a manager of all of the keys in that key ring.
{: note}

The following table shows how service access roles map to {{site.data.keyword.keymanagementserviceshort}} permissions.

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a key | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Import a key | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Retrieve a key | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Retrieve key metadata | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Retrieve key total | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List keys | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Wrap a key | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Unwrap a key | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Rewrap a key | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Rotate a key | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Disable a key | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Enable a key | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Schedule deletion for a key | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Cancel deletion for a key | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Delete a key | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Restore a key | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Patch a key | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: #table-2}
{: caption="Table 2. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key resources" caption-side="top"}
{: tab-title="Keys"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a key ring | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List key rings | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Delete a key ring | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: #table-3}
{: caption="Table 3. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key ring resources" caption-side="top"}
{: tab-title="Key Rings"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Set key policies | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List key policies | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Set instance policies | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List instance policies | | | | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: #table-3}
{: caption="Table 4. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} policy resources" caption-side="top"}
{: tab-title="Policies"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create an import token | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Retrieve an import token | | | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: #table-4}
{: caption="Table 5. Lists service access roles as they apply to import token resources" caption-side="top"}
{: tab-title="Import tokens"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a registration[^services-1] | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List registrations for a key | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| List registrations for any key | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Update a registration[^services-2] | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Replace a registration[^services-3] | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
| Delete a registration[^services-4] | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) | ![Check mark icon](..images/icons/checkmark-icon.svg) |
{: #table-5}
{: caption="Table 6. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} registration resources" caption-side="top"}
{: tab-title="Registrations"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

### Roles and {{site.data.keyword.iamshort}} policies
{: #manage-access-roles-policies}

While the {{site.data.keyword.keymanagementserviceshort}} console allows users fine-grained access control using these roles, it can be helpful to remember that these roles are attached to {{site.data.keyword.iamshort}} policies:

* service name (for Key Protect, always `kms`)
* service instance id
* key ring id
* resource type (only `key` is supported)
* resource id
* account id (should always specified in policy)

Here is an example of a policy returned by the IAM API:

```
"resources": [
    {
        "attributes": [
            {
                "name": "accountId",
                "value": "$ACCOUNT_ID",
            },
            {
                "name": "serviceName",
                "value": "kms",
            },
            {
                "name": "resourceType",
                "value": "key",
            },
            {
                "name": "resource",
                "value": "$KEY_ID",
            },
            {
                "name": "keyRing",
                "value": "$KEY_RING_ID",
            }
        ]
    }
]
```

Any combination of these attributes can be applied in a policy. If that policy has the administrator role attached to it, that means any `user/service id/access group` that has this policy applied to them can create a policy that applies to a subresource of the one that has been been granted. In other words, all sub-admin users can only have access equal to (exactly the same attributes specified on their policy) or less than (exactly the same attributes specified on their policy and additional attributes specified) that of the parent admin.

## What's next
{: #manage-access-next-steps}

Account owners and admins can invite users and set service policies that correspond to the {{site.data.keyword.keymanagementserviceshort}} actions the users can perform.

- For more information about assigning user roles in the {{site.data.keyword.cloud_notm}} UI, see [Managing IAM access](/docs/account?topic=account-account-getting-started){: external}.
