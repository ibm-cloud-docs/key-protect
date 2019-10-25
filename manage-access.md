---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-24"

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

# Managing user access
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} supports a centralized access control system, governed by {{site.data.keyword.iamlong}}, to help you manage users and access for your encryption keys.
{: shortdesc}

A good practice is to grant access permissions as you invite new users to your account or service. For example, consider the following guidelines:

- **Enable user access to the resources in your account by assigning Cloud IAM roles.**
    Rather than sharing your admin credentials, create new policies for users who need access to the encryption keys in your account. If you are the admin for your account, you are automatically assigned a _Manager_ policy with access to all resources under the account.
- **Grant roles and permissions at the smallest scope needed.**
    For example, if a user needs to access only a high-level view of keys within a {{site.data.keyword.keymanagementserviceshort}} service instance, grant the _Reader_ role to the user for that instance. You can also [assign fine-grained access to a single key](/docs/services/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level). 
- **Regularly audit who can manage access control and delete key resources.**
    Remember that granting a _Manager_ role to a user means that the user can modify service policies for other users, in addition to destroying resources.

## Roles and permissions
{: #roles}

With {{site.data.keyword.iamshort}} (IAM), you can manage and define access for users and resources in your account.

To simplify access, {{site.data.keyword.keymanagementserviceshort}} aligns with Cloud IAM roles so that each user has a different view of the service, according to the role the user is assigned. If you are a security admin for your service, you can assign Cloud IAM roles that correspond to the specific {{site.data.keyword.keymanagementserviceshort}} permissions you want to grant to members of your team.

This section discusses {{site.data.keyword.cloud_notm}} IAM in the context of {{site.data.keyword.keymanagementserviceshort}}. For complete IAM documentation, see [Managing access in {{site.data.keyword.cloud_notm}}](/docs/iam?topic=iam-cloudaccess).
{: note}

### Platform access roles
{: #platform-mgmt-roles}

Use platform access roles to grant permissions at the account level, such as the ability to create or delete instances in your {{site.data.keyword.cloud_notm}} account.

| Action | Role |
| --- | --- |
| View {{site.data.keyword.keymanagementserviceshort}} instances | Administrator, Operator, Editor, Viewer |
| Create {{site.data.keyword.keymanagementserviceshort}} instances | Administrator, Editor |
| Delete {{site.data.keyword.keymanagementserviceshort}} instances | Administrator, Editor |
{: caption="Table 1. Lists platform management roles as they apply to {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

### Service access roles
{: #service-access-roles}

Use service access roles to grant permissions at the service level, such as the ability to view, create, or delete {{site.data.keyword.keymanagementserviceshort}} keys. 

The following table shows how service access roles map to {{site.data.keyword.keymanagementserviceshort}} permissions.

<table>
  <col width="20%">
  <col width="40%">
  <col width="40%">
  <tr>
    <th>Role</th>
    <th>Description</th>
    <th>Actions</th>
  </tr>
  <tr>
    <td><p>Reader</p></td>
    <td><p>A reader can browse a high-level view of keys and perform wrap and unwrap actions. Readers cannot access or modify key material.</p></td>
    <td>
      <p>
        <ul>
          <li>View keys</li>
          <li>Wrap keys</li>
          <li>Unwrap keys</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Writer</p></td>
    <td><p>A writer can create keys, modify keys, rotate keys, and access key material.</p></td>
    <td>
      <p>
        <ul>
          <li>Create keys</li>
          <li>View keys</li>
          <li>Rotate keys</li>
          <li>Wrap keys</li>
          <li>Unwrap keys</li>
          <li>Create import tokens</li>
          <li>Retrieve import tokens</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Manager</p></td>
    <td><p>A manager can perform all actions that a reader and writer can perform, including the ability to set rotation policies for keys, delete keys, invite new users, and assign access policies for other users.</p></td>
    <td>
      <p>
        <ul>
          <li>All actions that a reader or a writer can perform</li>
          <li>Assign user access policies</li>
          <li>Set key rotation policies</li>
          <li>Delete keys</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Table 1. Describes how service access roles map to {{site.data.keyword.keymanagementserviceshort}} permissions</caption>
</table>

## What's next
{: #manage-access-next-steps}

Account owners and admins can invite users and set service policies that correspond to the {{site.data.keyword.keymanagementserviceshort}} actions the users can perform.

- For more information about assigning user roles in the {{site.data.keyword.cloud_notm}} UI, see [Managing IAM access](/docs/iam?topic=iam-getstarted){: external}.

