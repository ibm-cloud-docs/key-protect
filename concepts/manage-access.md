---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-20"

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

## How IAM access works
{: #grant-access-keys-how-access}

After you set up and organize resource groups in your account, you can take advantage of a couple of strategies to streamline the access management process:

**Access groups**  
* You can minimally manage the number of assigned policies by giving the same access to all identities in an access group instead of assigning the same access multiple times per individual user, service ID, or trusted profile. Users must be invited to your account before you can add them to an access group. If a user qualifies for a trusted profile that is a member of the access group, you don't need to invite them to your account.

**Trusted profiles**  
* If your organization has an enterprise directory, trusted profiles can reduce the time and effort to manage access. It simplifies the login process to your {{site.data.keyword.cloud_notm}} account for federated users in your enterprise. You can automatically grant federated users or compute resources access to your account by creating trusted profiles. For federated users, add conditions based on SAML attributes to define which federated users can apply a profile. For compute resources, specify specific resources, or add conditions based on resource attributes to define which compute resources can apply a profile. For both entity types, the level of access that is granted is determined by the access policies that are specified within each trusted profile, or the access groups that the trusted profile is a member of. However, trusted profiles don't require federated users to be invited to an account, and only users that are federated by an external identity provider (IdP) can apply a trusted profile.

When you're a member of multiple access groups, all policies apply at once when you access an account. As a federated user, you might have the option to apply different trusted profiles, but you select just one profile to apply when you log in. For example, if you want to complete developer-related tasks, select the `Developer` profile when logging in. If you want to complete an administrator-related task, you select the `Admin` profile that has privileged permissions. This way, you reduce the risk of taking privileged actions by mistake.
{: tip}

A policy consists of a subject, target, and role. The subject in this case is the access group or trusted profile. The target is what you want the subject to access, such as a set of resources in a resource group, a service instance, all services in the account, or all instances of a service. The role defines the level of access that is granted.

For more information about how platform and services roles work in {{site.data.keyword.keymanagementserviceshort}}, check out [Platform roles and service roles](#manage-access-roles).

### Best practices
{: #manage-access-best-practices}

There is a [limit](/docs/account?topic=account-known-issues#iam_limits) on the total number of policies that are allowed in an account. You can use a few strategies to ensure that you don't reach the limit and to reduce the amount of time that you spend managing access for the identities in your account (users, service IDs, or trusted profiles):

* Use the principle of least privilege and assign only the access that is necessary. This can help you ensure that the identities in your account are limited to only the actions that you want to allow. For example, rather than the account owner sharing their credentials, which by default give them _Administrator_ and _Manager_ access over all resources under their account, create new policies for users who need access to the account and to each service instance (and its associated keys).
* Add resources to a resource group to further minimize the number of necessary policies. For example, you might have a team working on a project that uses specific resources in your account. Add the team members to an access group or trusted profile with a policy that assigns access to only the resources that are in a specific resource group. This way, you don't need to assign a policy to each resource for each team member. For more information about assigning fine-grained access, check out [assign fine-grained access to a single key](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level).
* Use access groups to streamline managing access for identities that require the same level of access. You can set up an access group with a specific policy defined, and then add those identities to the group. If the group members need more access later on, you simply define a new policy for the access group.
* Use access management tags to control access to the resources in your account at scale. By assigning access only to resources that have specific tags that are attached to them, you can avoid multiple updates to your defined policies. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
* Use trusted profiles to automatically grant federated users and compute resources access to your account. This way, federated users can be mapped to one or more trusted profiles during login by evaluating SAML-based attributes to determine which profiles they can apply. Using trusted profiles for compute resources helps you avoid storing credentials to run applications and the management and rotation of credentials. You can also add trusted profiles to access groups to leverage the set of policies you have already created.
* Regularly audit who can manage access control and delete key resources. Because the errors and misuse by users with elevated permissions can cause damage to your account, your service instances, and data protected by keys in your service instance, ensure that proper access is being maintained by auditing the users with those roles. Remember that any new keys, key rings, or service instances created are subject to existing role definitions by default. If a new key should not be accessed or modified by a new user being assigned as a _Manager_ of the instance, that restriction must be specifically assigned, as instance managers have access to all of the keys in an instance by default, including the ability to delete a key.

### What makes a good access group strategy?
{: #grant-access-keys-accessgroup-strategy}

An access group is an organization of users, service IDs, and trusted profiles in a grouping that you can grant the same IAM access. All identities in a single access group inherit the same access.

A logical way to assign access to your resource groups and the included resources is by [creating one access group](/docs/account?topic=account-groups) per required level of access. Then, you can map each access group to the previously created resource groups. For example, to control access to the `CustApp` project, you might create the following access groups:

* Auditor-Group
* Developer-Group
* Admin-Group

For the Auditor-Group, assign two access policies that grant viewer access to the `CustApp-Test` and the `CustApp-Prod` resources and resource groups. For the Developer-Group, assign two access policies that grant editor access to the `CustApp-Dev` and `CustApp-Test` resources and resource groups. For the Admin-Group, assign three access policies that grant administrator access to all three `CustApp` resource groups and their resources.

You can assign administrator access to everything in an account by creating an access group and assigning two policies to it. To create the first policy, select **All Identity and Access enabled services** in **Account** with the Administrator platform role and Manager service role. To create the second policy, select **All Account Management Services** with the Administrator role assigned.
{: tip}

For more best practices from IBM Garage for Cloud, see [Managing access to resources in {{site.data.keyword.cloud_notm}}](https://cloudnativetoolkit.dev/resources/ibm-cloud/access-control/){: external}.

## Platform roles and service roles
{: #manage-access-roles}

The word "object" is used in this section as a broad term for things like keys or key rings or service instances or accounts.
{: important}

As mentioned earlier, roles exist at both the platform (account) and service level. If you are unsure about what a platform or a service role allows a user to do, remember that platform roles interact mainly with {{site.data.keyword.cloud_notm}} services like the [resource controller](/docs/account?topic=account-overview) or {{site.data.keyword.iamshort}}. Roles inside of a service, on the other hand, interact mainly with the relevant API, which in this case is the {{site.data.keyword.keymanagementserviceshort}} API. This is why, as you'll see, platform roles have limited use inside of your service instances beyond (in the case of the _Administrator_ role) the ability to create an access policy for a particular object, such as a key ring.

**Platform roles**

* **Administrator**: Has the full spectrum of rights over a particular object and its "child" objects (for example, keys are child objects of instances), including the right to invite new users and assign roles over the object (only administrators can assign roles). Note that administrators do not have service roles by default. They can, however, assign roles to themselves.
* **Editor**: Can view, create, and delete instances at the account level, but cannot invite new users. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.
* **Operator**: Can view instances at the account level, but cannot edit them. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.
* **Viewer**: Can view instances at the account level, but cannot edit them. Has limited use for objects within a service instance, such as keys, beyond the ability to view them.

Platform roles be assigned over an entire account, over particular service instances, or within objects inside of a service instance.

| Action | Viewer | Editor | Operator | Administrator |
| ------ | ------ | ------ | -------- | ------------- |
| View {{site.data.keyword.keymanagementserviceshort}} instances | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) |
| Create {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](../../icons/checkmark-icon.svg) | | ![Check mark icon](../../icons/checkmark-icon.svg) |
| Delete {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](../../icons/checkmark-icon.svg) | | ![Check mark icon](../../icons/checkmark-icon.svg) |
| Invite new users and manage access policies | | | | ![Check mark icon](../../icons/checkmark-icon.svg) |
{: caption="Table 1. Lists platform management roles as they apply to {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

While an account-level role gives a user particular permissions over service instances by default, roles can also be assigned over a particular service instance. For example, an account _Editor_ (who has the ability to view, create, and delete instances, but not the ability to assign roles) can be made an _Administrator_ of a particular service instance, allowing them to assign roles within that service instance.

Service roles can be applied to the three first class objects within a service instance: the **instance** as a whole, particular **keys**, and **key rings**. Just as account roles have permissions over instances by default, so too do instance managers have permissions over keys and key rings by default. However, these permissions can be assigned more granularly where necessary, for example giving a user the _Manager_ role over only a particular key or key ring and some lesser level of permission over the instance as a whole.

Service roles can be assigned per-instance or for all instances in an account.
{: tip}

**Service instance roles**

* **Manager**: Has the full spectrum of rights over a particular object (for example, the manager of a key has the ability to wrap, unwrap, and delete the key, as well as the exclusive right to read and update {{site.data.keyword.keymanagementserviceshort}} policies such as `dualAuthDelete`, `allowedNetwork`, `allowedIP`, among others).
* **Writer**: Has most of the same rights a manager does when it comes to using an object (including the ability to retrieve a key and its metadata), but generally cannot delete or disable the object.
* **Reader**: Can use the object (for example, key readers can wrap and unwrap a key), but neither create, delete, or modify the object.
* **ReaderPlus**: Have the same rights as a reader, with the additional ability to retrieve a standard key's payload.
* **KeyPurge**: Has the ability to [purge keys after four hours](/docs/key-protect?topic=key-protect-delete-purge-keys).

Note that the permissions included in roles are **additive**. A _Manager_, for example, has all of the permissions that a _Reader_ has and more. The exception is the _KeyPurge_ role, which includes the `kms.secrets.purge` action that is not a part of any other role and must therefore be set explicitly.
{: note}

The following table shows how service access roles map to {{site.data.keyword.keymanagementserviceshort}} permissions.

| Action | Reader | ReaderPlus | Writer | Manager | KeyPurge |
| ------ | ------ | ---------- | ------ | ------- | -------- |
| Create a key | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Import a key | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Retrieve a key | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Retrieve key metadata | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Retrieve key total | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List keys | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List key versions | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Wrap a key | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Unwrap a key | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Rewrap a key | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Rotate a key | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Disable a key | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Enable a key | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Schedule deletion for a key | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Cancel deletion for a key | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Delete a key | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Restore a key | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Patch a key | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Sync keys | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Purge keys after four hours | | | | | ![Check mark icon](../../icons/checkmark-icon.svg) |
{: #table-2}
{: caption="Table 2. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key resources" caption-side="top"}
{: tab-title="Keys"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager | KeyPurge |
| ------ | ------ | ---------- | ------ | ------- | -------- |
| Create a key ring | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List key rings | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Delete a key ring | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
{: #table-3}
{: caption="Table 3. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key ring resources" caption-side="top"}
{: tab-title="Key Rings"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager | KeyPurge |
| ------ | ------ | ---------- | ------ | ------- | -------- |
| Set key policies | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List key policies | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Set instance policies | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List instance policies | | | | ![Check mark icon](../../icons/checkmark-icon.svg) | |
{: #table-4}
{: caption="Table 4. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} policy resources" caption-side="top"}
{: tab-title="Policies"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager | KeyPurge |
| ------ | ------ | ---------- | ------ | ------- | -------- |
| Create an import token | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Retrieve an import token | | | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
{: #table-5}
{: caption="Table 5. Lists service access roles as they apply to import token resources" caption-side="top"}
{: tab-title="Import tokens"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager | KeyPurge |
| ------ | ------ | ---------- | ------ | ------- | -------- |
| Create a registration[^services-1] | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List registrations for a key | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| List registrations for any key | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Update a registration[^services-2] | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Replace a registration[^services-3] | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
| Delete a registration[^services-4] | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | ![Check mark icon](../../icons/checkmark-icon.svg) | |
{: #table-6}
{: caption="Table 6. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} registration resources" caption-side="top"}
{: tab-title="Registrations"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

The _KeyPurge_ role only confers the ability to purge keys and should be considered additive to other service access roles, such as _Manager_.
{: note}

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
