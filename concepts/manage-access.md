---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-11"

keywords: user permissions, manage access, IAM roles

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

# Managing user access
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} supports a centralized access
control system, governed by {{site.data.keyword.iamlong}}, to help you manage
users and access for your encryption keys.
{: shortdesc}

A good practice is to grant access permissions as you invite new users to your
account or service. For example, consider the following guidelines:

- **Enable user access to the resources in your account by assigning Cloud IAM roles.**
  Rather than sharing your admin credentials, create new policies for users who
  need access to the encryption keys in your account. If you are the admin for
  your account, you are automatically assigned a _Manager_ policy with access
  to all resources under the account.

- **Grant roles and permissions at the smallest scope needed.**
  For example, if a user needs to access only a high-level view of keys within a
  {{site.data.keyword.keymanagementserviceshort}} instance, grant the
  _Reader_ role to the user for that instance. You can also
  [assign fine-grained access to a single key](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level).

- **Regularly audit who can manage access control and delete key resources.**
  Remember that granting a _Manager_ role to a user means that the user can
  modify service policies for other users, in addition to destroying resources.

## Roles and permissions
{: #roles}

With {{site.data.keyword.iamshort}} (IAM), you can manage and define access for
users and resources in your account.

To simplify access, {{site.data.keyword.keymanagementserviceshort}} aligns with
Cloud IAM roles so that each user has a different view of the service, according
to the role the user is assigned. If you are a security admin for your service,
you can assign Cloud IAM roles that correspond to the specific
{{site.data.keyword.keymanagementserviceshort}} permissions you want to grant to
members of your team.

This section discusses {{site.data.keyword.cloud_notm}} IAM in the context of
{{site.data.keyword.keymanagementserviceshort}}. For complete IAM documentation,
see
[Managing access in {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-cloudaccess){: external}.
{: note}

### Platform access roles
{: #platform-mgmt-roles}

Use platform access roles to grant permissions at the account level, such as the
ability to create or delete instances in your {{site.data.keyword.cloud_notm}}
account.

| Action | Viewer | Editor | Operator | Administrator |
| ------ | ------ | ------ | -------- | ------------- |
| View {{site.data.keyword.keymanagementserviceshort}} instances | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Create {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](../icons/checkmark-icon.svg) | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Delete {{site.data.keyword.keymanagementserviceshort}} instances | | ![Check mark icon](../icons/checkmark-icon.svg) | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Invite new users and manage access policies | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
{: caption="Table 1. Lists platform management roles as they apply to {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

If you're an account owner, you are automatically assigned _Administrator_
platform access to your {{site.data.keyword.keymanagementserviceshort}} service
instances so you can further assign roles and customize access policies for
others.
{: note}

### Service access roles
{: #service-access-roles}

Use service access roles to grant permissions at the service level, such as the
ability to view, create, or delete
{{site.data.keyword.keymanagementserviceshort}} keys.

- As a **Reader**, you can browse a high-level view of keys and perform wrap and
  unwrap actions. Readers cannot access or modify key material.

- As a **ReaderPlus**, you can browse a high-level view of keys, access key
  material for standard keys, and perform wrap and unwrap actions. The
  ReaderPlus role cannot modify key material.

- As a **Writer**, you can create keys, modify keys, rotate keys, and access key

- As a **Manager**, you can perform all actions that a Reader, ReaderPlus and
  Writer can perform, including the ability to delete keys and set dual
  authorization and rotation policies for keys.

The following table shows how service access roles map to
{{site.data.keyword.keymanagementserviceshort}} permissions.

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a key | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Import a key | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Retrieve a key | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Retrieve key metadata | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Retrieve key total | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| List keys | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Wrap a key | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Unwrap a key | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Rewrap a key | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Rotate a key | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Disable a key | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Enable a key | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Schedule deletion for a key | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Cancel deletion for a key | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Delete a key | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Restore a key | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Patch a key | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
{: #table-2}
{: caption="Table 2. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key resources" caption-side="top"}
{: tab-title="Keys"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a key ring | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| List key rings | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Delete a key ring | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
{: #table-3}
{: caption="Table 3. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} key ring resources" caption-side="top"}
{: tab-title="Key Rings"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Set key policies | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| List key policies | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| Set instance policies | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
| List instance policies | | | | ![Check mark icon](../icons/checkmark-icon.svg) |
{: #table-3}
{: caption="Table 4. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} policy resources" caption-side="top"}
{: tab-title="Policies"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create an import token | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Retrieve an import token | | | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
{: #table-4}
{: caption="Table 5. Lists service access roles as they apply to import token resources" caption-side="top"}
{: tab-title="Import tokens"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

| Action | Reader | ReaderPlus | Writer | Manager |
| ------ | ------ | ---------- | ------ | ------- |
| Create a registration[^services-1] | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| List registrations for a key | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| List registrations for any key | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Update a registration[^services-2] | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Replace a registration[^services-3] | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
| Delete a registration[^services-4] | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) | ![Check mark icon](../icons/checkmark-icon.svg) |
{: #table-5}
{: caption="Table 6. Lists service access roles as they apply to {{site.data.keyword.keymanagementserviceshort}} registration resources" caption-side="top"}
{: tab-title="Registrations"}
{: tab-group="IAM-roles"}
{: class="comparison-tab-table"}

[^services-1]: This action is performed on your behalf by an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
that has enabled support for key registration.
[Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-2]: This action is performed on your behalf by an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
that has enabled support for key registration.
[Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-3]: This action is performed on your behalf by an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
that has enabled support for key registration.
[Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-4]: This action is performed on your behalf by an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
that has enabled support for key registration.
[Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

## What's next
{: #manage-access-next-steps}

Account owners and admins can invite users and set service policies that
correspond to the {{site.data.keyword.keymanagementserviceshort}} actions the
users can perform.

- For more information about assigning user roles in the
  {{site.data.keyword.cloud_notm}} UI, see
  [Managing IAM access](/docs/account?topic=account-account-getting-started){: external}.
