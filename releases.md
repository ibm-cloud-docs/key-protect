---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-16"

keywords: release notes, service updates, service bulletin

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

# What's new
{: #releases}

Stay up-to-date with the new features that are available for
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

## August 2021
{: #august-2021}

### Added: Updates to the Key Protect console and Key purge feature
{: #key-purge-august-2021}

New as of: 2021-08-05

The ability to purge keys that have been deleted after four hours using the UI has been added. For more information, check out [About purging and deleting keys](/docs/key-protect?topic=key-protect-delete-purge-keys#delete-purge-keys-considerations).

Also, the ability to view your instance ID and cloud resource name (CRN) has been made easier. For more information, check out [Retrieving your instance ID and cloud resource name (CRN)](/docs/key-protect?topic=key-protect-retrieve-instance-ID).

## June 2021
{: #june-2021}

### Added: Updates to the Key Protect CLI plugin
{: #added-ui-updates-june-2021}

New as of: 2021-06-30

The {{site.data.keyword.keymanagementserviceshort}} CLI plugin has been updated to version 0.6.3. Minor changes in this [release](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference) include support for {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.hscrypto}} specific algorithms in [Key Create](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-create) and [Key Rotate](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference#kp-key-rotate).

## April 2021
{: #april-2021}

### Added: Updates to the Key Protect console and Key purge feature
{: #added-ui-updates-april-2021}

New as of: 2021-04-22

Many of the panels and actions you can take in the console have been modified to accommodate the addition of key rings and add other functionality to the console that previously was only possible using the APIs. This includes a new **Key rings** panel where key rings can be managed and a **Key ring ID** column in the **Keys** panel.

Note that as part of this change, the **Instance policies** and **Manage keys** panels in the console have been renamed **Instance policies** and **Keys** respectively.

This release also adds the ability to use the API to purge keys four hours after they have been moved to the _Destroyed_ state. For more information, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).

The ability to purge keys using the UI was added in the [August, 2021 release](#key-purge-august-2021).

## March 2021
{: #march-2021}

### Added: Transfer a key to a different key ring
{: #added-transfer-key-key-ring}

New as of: 2021-03-12

You can now use the {{site.data.keyword.keymanagementserviceshort}} API to transfer
a key from one key ring to another key ring. In order to move a key, you must have
_Manager_ IAM access permissions to both the key and the target key ring, which is the
key ring that you would like the key to be transferred to.

To find out more, see
[Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys#transfer-key-key-ring-api).

### Added: Major Changes to the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in.
{: #added-cli-changes}

Added support for aliases and key-rings bring best practices to using the [{{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference). Learn more about all of the new features in the [CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## February 2021
{: #february-2021}

### Added: Functionality to key restoration
{: 2021-02-25-key-restore}

New as of: 2021-02-25

The process to restore keys has been enhanced:

* Deleted keys can now be restored up to 30 days after deletion. After 30 days, it is no longer possible to restore a key.
* Any and all types of keys (standard or root, imported or created) can be restored.
* It is no longer required to pass in key material when restoring a key.

For more information, check out [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys).

### Added: Updates to search by extractable content
{: #added-search-extractable}

Rotating keys and re-wrapping encrypted content with a new DEK is fundamental to security. Quickly retrieve keys having extractable content with [this search feature](/docs/key-protect?topic=key-protect-view-keys#filter-keys-extractable-state-api).

### Added: Updates to the Key Protect UI
{: #upcoming-key-encrypt-ui}

New as of: 2021-02-15

Managing keys through the UI interface has been enhanced with new options presented in a simple and convenient selectable option menu within the context of each managed key. Simply click on the "overflow" icon  at the end of each row to access common features and UI enhancements.

Note that "Associated Resources" are now accessible within the context of each key in the console, as well as having its own item in the menu. Also, setting a key's rotation policy is now possible in the same panel where a key can be manually rotated.

Also, the list of endpoints in the console has grown with the addition of Osaka. You can now update your applications to reference the new endpoint.

### Added: Feature updates to the Key Protect UI
{: #upcoming-key-encrypt-ui}

New as of: 2021-02-15

The Key Protect UI now has support for the following feature:

Wrap/Unwrap (Envelope Encryption)
You can now use the Key Protect UI to wrap/unwrap active root keys. For more information, see Protecting data with envelope encryption for an overview, or check the documentation for how to wrap and unwrap your keys.

### Coming soon: Updates to key deletion functionality
{: #upcoming-key-deletion-changes}

Release Date: 2021-03-15

Beginning in April 2021, {{site.data.keyword.keymanagementserviceshort}}
will implement a key purge feature that will automatically purge any keys that
have been deleted for more than 90 days. A purged key and its associated data
will be permanently removed, or hard deleted, from the
{{site.data.keyword.keymanagementserviceshort}} database.

When a user or service deletes a key in Key Protect today, if the key is not
restored within 30 days, the key is soft deleted. All key data, except key
material data, remains in {{site.data.keyword.keymanagementserviceshort}}, and
those details are retrievable by List Keys, Get Key and other APIs. Once
automatic key purge is introduced, users will not be able to retrieve any
information regarding a purged key. Any API calls that use the Key ID of a
purged key will result in a 404 HTTP Not Found error.

Note: A key purge can be reversed by restoring the hard deleted key.

- **How will the changes impact my environment?**
    The majority of users will not notice an impact. Please note that any data
    related to a purged key (key metadata, registrations, policies, etc) will no
    longer be available via the {{site.data.keyword.keymanagementserviceshort}}
    service. If you are required to retain any data related to a purged key
    (key metadata, registrations, policies, etc) for an extended period of time,
    it is recommended to perform the necessary API or CLI calls to retrieve and
    store that data in your own storage device.

### Added: Updates to the {{site.data.keyword.keymanagementserviceshort}} UI
{: #february-2021-ui-updates}

New as of: 2021-02-05

If you have _Manager_ access permissions, you can filter for keys in the
**Destroyed** state and restore an imported root key via the ⋯ icon on the
**Keys** table. You can use the restore key side panel to complete the
process for restoring the key.

For more information, see [Restoring a deleted key with the console](/docs/key-protect?topic=key-protect-restore-keys#restore-ui).

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for virtual private clouds
{: #added-private-endpoints}

New as of: 2021-02-05

You can now connect to {{site.data.keyword.keymanagementserviceshort}} from your
virtual private cloud(VPC) via virtual private endpoint. VPEs are bound to a
[VPE gateway](/docs/vpc?topic=vpc-about-vpe) and serve as an intermediary
that enables your workload to interact with
{{site.data.keyword.keymanagementserviceshort}}.

To get started, [provision a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started){: external}
and create a [VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external}. For more information, see
[Using private endpoints](/docs/key-protect?topic=key-protect-private-endpoints).

## January 2021
{: #january-2021}

### Added: Create a key ring
{: #added-key-ring}

New as of: 2021-01-27

You can now use the {{site.data.keyword.keymanagementserviceshort}} REST API
to manage access to a specific set of keys that are bundle a collection
called a `key ring`. You can manage and restrict access to key rings to via
IAM policies.

To find out more, see
[Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).

### Updated: Restore a deleted key
{: #updated-restore-key}

New as of: 2021-01-06

Restore key improved: If you have _Manager_ access permissions, you can
now use the the {{site.data.keyword.keymanagementserviceshort}} UI to restore all
unexpired-deleted keys.

For more information, see [Restoring
keys](/docs/key-protect?topic=key-protect-restore-keys#restore-ui).

### Added: Manual Data Synchronization
{: #added-manual-data-synchronization}

You can now use the {{site.data.keyword.keymanagementserviceshort}} REST API
to initiate a manual data synchronization request to to synchronize your service's
key records with what is in {{site.data.keyword.keymanagementserviceshort}}'s database
records.

For more information, see
[Sync associated resources](/docs/key-protect?topic=key-protect-sync-associated-resources){: external}.

This API is available only for users if a cloud service has enabled
the key registiation feature as part of its integration with
{{site.data.keyword.keymanagementserviceshort}}. To learn if an
[integrated
service](/docs/key-protect?topic=key-protect-integrate-services)
supports key registration, refer to its service documentation for
more information.
{: preview}

## December 2020
{: #december-2020}

### Added: Create a metrics policy
{: #added-metrics-policy}

New as of: 2020-12-11

If you have _Manager_ IAM access permissions, you can now use the
{{site.data.keyword.keymanagementserviceshort}} UI to create a metrics policy
that allows you to view the operational metrics for your
{{site.data.keyword.keymanagementserviceshort}} instance.

To find out more, see
[Managing metrics](/docs/key-protect?topic=key-protect-manage-monitor-metrics#enable-metrics-instance-policy-ui).

### Added: Create a key alias
{: #added-key-alias}

New as of: 2020-12-01

If you have _Writer_ or _Manager_ access permissions, you can now use the
{{site.data.keyword.keymanagementserviceshort}} REST API to create a key alias.
You can use a key alias to refer to a key in your
{{site.data.keyword.keymanagementserviceshort}} service instance.

To find out more, see
[Creating key aliases](/docs/key-protect?topic=key-protect-create-key-alias).

## October 2020
{: #october-2020}

### Added: Quantum Safe Cryptography in TLS Connections
{: #added-quantum-safe-cryptography}

New as of: 2020-10-20

In preparation for the post-quantum era, you can use a quantum safe enabled TLS
connection to secure your communication to the
{{site.data.keyword.keymanagementservicefull}} service.

To find out more, see
[Using Quantum Safe Cryptography](/docs/key-protect?topic=key-protect-quantum-safe-cryptography-tls-introduction).

### Added: Manage key creation and importation policies in the UI
{: #added-key-creation-importation-policies-ui}

New as of: 2020-10-12

You can set a key creation and importation policy, in the
[user interface (UI)](https://{DomainName}/){: external},
to restrict how keys are created and imported into your
{{site.data.keyword.keymanagementserviceshort}} service instance.

See the updated "Instance policies" pane in the
{{site.data.keyword.keymanagementserviceshort}} UI.

## September 2020
{: #september-2020}

### Added: Manage key creation and importation policies in the API
{: #added-key-creation-importation-policies-api}

New as of: 2020-09-17

You can set a key creation and importation policy, using the
[API](/apidocs/key-protect){: external},
to restrict how keys are created and imported into your
{{site.data.keyword.keymanagementserviceshort}} service instance.

To find out more, see
[Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess).

### Added: Feature updates to the {{site.data.keyword.keymanagementserviceshort}} UI
{: #september-2020-ui-updates}

New as of: 2020-09-22

The {{site.data.keyword.keymanagementserviceshort}} UI now has support for the
following feature:

- List keys by key state: You can now use the
    {{site.data.keyword.keymanagementserviceshort}} UI to filter and retrieve keys
    that are in a specified state. For more information, see
    [Viewing keys in the console](/docs/key-protect?topic=key-protect-view-keys#filter-key-state-gui).

If you have _Manager_ access permissions, you can filter for keys in the
**Destroyed** state and restore an imported root key via the ⋯ icon on the
**Keys** table. Note: you must include the original Key Material to restore the
key.

### Update: CLI plug-in Version 0.5.2 is now available
{: #september-2020-cli-plugin-052-available}

New as of: 2020-09-09

The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in version 0.5.2
was updated with these changes:

- Commands that specify JSON outout (`--output json`) now return an empty JSON
    structure if there is no output.

The
[CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog)
has all CLI updates.

## July 2020
{: #july-2020}

### Update: CLI plug-in Version 0.5.1 is now available
{: #july-2020-cli-plugin-051-available}

New as of: 2020-07-21

The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is used to
manage keys in your instance.

To install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see
[setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli).

For a detailed explanation of changes in version 0.5.1, see the
[CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## June 2020
{: #june-2020}

### Added: Feature updates to the {{site.data.keyword.keymanagementserviceshort}} UI
{: #june-2020-ui-updates}

New as of: 2020-06-24

The {{site.data.keyword.keymanagementserviceshort}} UI now has support for the
following features:

- Enable/disable key: If you have _Manager_ access permissions, you can now use
    the {{site.data.keyword.keymanagementserviceshort}} UI to suspend or restore a
    key's encrypt and decrypt operations. For more information, see
    [Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys#disable-enable-ui)

- Restore key: If you have _Manager_ access permissions, you can now use the the
    {{site.data.keyword.keymanagementserviceshort}} UI to restore a previously
    imported root key that was deleted. For more information, see
    [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys#restore-ui).

- Set an instance level dual authorization policy: You can now use the
    {{site.data.keyword.keymanagementserviceshort}} UI to require two users to
    safely delete a key from your {{site.data.keyword.keymanagementserviceshort}}
    instance. For more information, see
    [Enabling a dual authorization policy for an instance](/docs/key-protect?topic=key-protect-manage-dual-auth#enable-dual-auth-instance-policy-ui).

- Set an instance level network policy: You can now use the
    {{site.data.keyword.keymanagementserviceshort}} UI to restrict requests to
    public or private networks. For more information, see
    [Managing Network Access Policies](/docs/key-protect?topic=key-protect-managing-network-access-policies#enabling-network-access-to-your-service-instance-ui).

### Update: CLI plug-in Version 0.5.0 is now available
{: #june-2020-cli-plugin-050-available}

New as of: 2020-06-19

The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is used to
manage keys in your instance.

To install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see
[setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli).

For a detailed explanation of changes in version 0.5.0, see the
[CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## May 2020
{: #may-2020}

### Coming soon: Updates to activity tracker fields
{: #upcoming-activity-tracker-changes}

Release Date: 2020-05-29

Beginning in late May 2020, {{site.data.keyword.keymanagementserviceshort}} will
return updated event fields in
[{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external}
logs. These updates will be available across all supported regions by 29 May
2020.

- Successful replace registration, update registration, and unwrap key events
    will change from severity level `warning` to `normal`.

- The `rewrapedKeyVersionId` field will change to `rewrappedKeyVersionId`.

- The `TotalResources` field will change to `totalResources`.

- **Why are we making these changes?**
    These changes are required to remove deprecated event fields and support
    upcoming service enhancements for {{site.data.keyword.at_full_notm}}.

- **How will the changes impact my environment?**
    This change impacts the event fields that are returned in
    [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external}
    audit logs when you perform {{site.data.keyword.keymanagementserviceshort}}
    actions. The change does not impact
    {{site.data.keyword.keymanagementserviceshort}} operations. As a security or
    compliance admin, ensure that the removed and changed event fields do not
    affect your audit operations.

### Added: Use an import token to rotate a key
{: #added-secure-rotate}

New as of: 2020-05-01

If you have _Writer_ or _Manager_ access permissions, you can now use the
{{site.data.keyword.keymanagementserviceshort}} REST API to rotate an root key
that was initially imported with an import token.

To find out more, see
[Using an import token to rotate a key](/docs/key-protect?topic=key-protect-rotate-keys#rotate-keys-secure-api).

### Added: Restore a deleted key
{: #added-restore-key}

New as of: 2020-05-01

If you have _Manager_ access permissions, you can now use the the
{{site.data.keyword.keymanagementserviceshort}} REST API to restore a previously
imported root key.

To find out more, see
[Restoring keys](/docs/key-protect?topic=key-protect-restore-keys).

### Added: Enable or disable a key
{: #added-enable-disable-key}

New as of: 2020-05-01

If you have _Manager_ access permissions, you can now use the the
{{site.data.keyword.keymanagementserviceshort}} REST API to suspend or restore a
keys encrypt and decrypt operations.

To find out more, see
[Disabling keys](/docs/key-protect?topic=key-protect-disable-keys).

## April 2020
{: #apr-2020}

### Added: Manage network access policies
{: #added-network-access-policies}

New as of: 2020-04-16

You can set a network access policy to allow API requests to a
{{site.data.keyword.keymanagementserviceshort}} instance from public or private
networks.

To find out more, see
[Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies).

## March 2020
{: #mar-2020}

### Added: View details about a key
{: #added-key-metadata}

New as of: 2020-03-14

If you have _Reader_ access permissions, you can now use the
{{site.data.keyword.keymanagementserviceshort}} REST API to view only details
about a specific standard key without retrieving the key itself.

To find out more, see
[Viewing details about a key](/docs/key-protect?topic=key-protect-retrieve-key-metadata).

### Added: View versions for a root key
{: #added-key-versions}

New as of: 2020-03-14

You can now audit the rotation history of a root key by viewing its key
versions. After you rotate a root key, the ID of the root key does not change,
but {{site.data.keyword.keymanagementserviceshort}} now returns key version
information to help you determine which version of the root key is protecting
your data.

To find out more, see
[Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).

## February 2020
{: #feb-2020}

### Changed: {{site.data.keyword.at_full_notm}} event fields
{: #changed-at-events}

New as of: 2020-02-28

Beginning in April 2020, {{site.data.keyword.keymanagementserviceshort}} will
return updated event fields in
[{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external}
logs. These updates will be available across all supported regions by 15 April
2020.

- **What's changing?**
    This change impacts the following {{site.data.keyword.at_full_notm}} event
    fields.

|Type of change|Affected event fields|
|--- |--- |
|Removed event fields|meta<br>observer.typeURI<br>requestHeader<br>requestPath<br>responseBody<br>type<br>typeURI|
|Changed event fields|The `eventTime` field will change from format `2020-02-03T20:20:37+0000` to `2020-02-03T20:20:37Z`.<br>The target.name field is currently set to {{site.data.keyword.keymanagementserviceshort}}. This value will change to the name of the resource on which the operation was performed. For example, the name of the encryption key, or the name of your {{site.data.keyword.keymanagementserviceshort}} instance.|
|New event fields|requestData<br>responseData|
{: caption="Describes upcoming {{site.data.keyword.at_full_notm}} event field changes." caption-side="top"}

- **Why are we making these changes?**
    These changes are required to remove deprecated event fields and support
    upcoming service enhancements for {{site.data.keyword.at_full_notm}}.

- **How will the changes impact my environment?**
    This change impacts the event fields that are returned in
    [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external}
    audit logs when you perform {{site.data.keyword.keymanagementserviceshort}}
    actions. The change does not impact
    {{site.data.keyword.keymanagementserviceshort}} operations. As a security or
    compliance admin, ensure that the removed and changed event fields do not
    affect your audit operations.

### Added: View associations between root keys and IBM Cloud resources
{: #added-registrations}

New as of: 2020-02-25

You can now use {{site.data.keyword.keymanagementserviceshort}} REST APIs to
examine which root keys are actively protecting what data so that you can
evaluate exposures based on your organization's security or compliance needs.

For more information, see
[View associations between root keys and {{site.data.keyword.cloud_notm}} resources](/docs/key-protect?topic=key-protect-view-protected-resources).

This extra feature is available only if a cloud service has enabled it as part
of its integration with {{site.data.keyword.keymanagementserviceshort}}. To
learn if an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
supports key registration, refer to its service documentation for more
information.
{: preview}

### Added: Prevent accidental or malicious deletion of keys
{: #added-prevent-key-deletion}

New as of: 2020-02-25

{{site.data.keyword.keymanagementserviceshort}} enabled extra security measures
to protect against the accidental or malicious deletion of keys.

- {{site.data.keyword.keymanagementserviceshort}} now blocks the deletion of a
    root key that's actively protecting a cloud resource. To learn if a key is
    registered to cloud resource, you can
    [review the resources](/docs/key-protect?topic=key-protect-view-protected-resources)
    that are associated with the key.

- You can now
    [force deletion on a key](/docs/key-protect?topic=key-protect-delete-keys#delete-keys-force-delete)
    that's protecting a cloud resource.

### Added: ReaderPlus service access role
{: #added-readerplus}

New as of: 2020-02-17

Need to grant read-only access to keys? You can now choose between the _Reader_
and _ReaderPlus_ IAM roles for better control over access to key material.

To learn more about service access roles, see
[Managing user access](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

### Changed: Extra response fields in existing APIs
{: #changed-extra-response-fields}

New as of: 2020-02-17

Effective 2020 February 24, {{site.data.keyword.keymanagementserviceshort}} will
return additional fields in the response bodies of some
{{site.data.keyword.keymanagementserviceshort}} REST APIs.

- **Why are we making these changes?**
    The extra response fields are required to support upcoming features and
    service enhancements.

- **How will the changes impact my environment?**
    These changes are backwards-compatible and affect only the response details
    for some API calls, including the create key, retrieve key, wrap key, unwrap
    key, and rewrap key actions. Customers and integrated services must ensure
    that the additional fields do not affect their operations.

## January 2020
{: #jan-2020}

### Added: Dual authorization policies for {{site.data.keyword.keymanagementserviceshort}} instances and keys
{: #added-dual-authorization}

New as of: 2020-01-15

You can now enable dual authorization policies to safely delete keys from your
{{site.data.keyword.keymanagementserviceshort}} instance. When you
enable dual authorization, you require an action from two users to delete a key.

- To learn how to enable dual authorization at the instance level, see
[Enabling a dual authorization policy for an instance](/docs/key-protect?topic=key-protect-manage-dual-auth).

- To learn how to enable dual authorization at the key level, see
[Enabling a dual authorization policy for a key](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy).

## November 2019
{: #nov-2019}

### Changed: Platform and service access roles
{: #changed-access-roles}

New as of: 2019-11-04

{{site.data.keyword.keymanagementserviceshort}} is updating its user access
roles and how they correspond to {{site.data.keyword.keymanagementserviceshort}}
service actions. Effective 13 November 2019,
{{site.data.keyword.keymanagementserviceshort}} will update access roles
according to the following table:

| Service action | Current role assignments | New role assignments |
| -------------- | ------------------------ | -------------------- |
| Create keys | Administrator, Editor, Writer, Manager | Writer, Manager |
| Retrieve a key by ID | Administrator, Editor, Writer, Manager | Writer, Manager |
| Retrieve a list of keys | Administrator, Editor, Writer, Manager, Viewer, Reader | Reader, Writer, Manager |
| Wrap keys | Administrator, Editor, Writer, Manager, Viewer, Reader | Reader, Writer, Manager |
| Unwrap keys | Administrator, Editor, Writer, Manager, Viewer, Reader | Reader, Writer, Manager |
| Rewrap keys | Administrator, Editor, Writer, Manager, Viewer, Reader | Reader, Writer, Manager |
| Rotate keys | Administrator, Editor, Writer, Manager | Writer, Manager |
| Set rotation policies | Administrator, Manager | Manager |
| Retrieve rotation policies | Administrator, Manager | Manager |
| Delete a key by ID | Administrator, Manager | Manager |
{: caption="Table 1. Lists upcoming changes to {{site.data.keyword.keymanagementserviceshort}} service access roles" caption-side="top"}

As an account owner or admin, review the existing access policies for all
{{site.data.keyword.keymanagementserviceshort}} users in your account to ensure
that they are assigned the appropriate levels of access.

To learn more about {{site.data.keyword.keymanagementserviceshort}} roles and
permissions, see
[Managing user access](/docs/key-protect?topic=key-protect-manage-access).

## September 2019
{: #sept-2019}

### Added: Fine-grained access to {{site.data.keyword.keymanagementserviceshort}} resources
{: #added-fine-grain-access}

New as of: 2019-09-27

As an account admin, you can now assign fine-grained access to individual keys
within a {{site.data.keyword.keymanagementserviceshort}} instance.

To learn more about granting access, see
[Granting access to keys](/docs/key-protect?topic=key-protect-grant-access-keys).

### Changed: Using import tokens to securely upload keys to {{site.data.keyword.keymanagementserviceshort}}
{: #added-import-tokens}

New as of: 2019-09-16

On 20 March 2019,
[{{site.data.keyword.keymanagementserviceshort}} announced transport keys](#added-transport-keys-beta)
as a beta feature for importing encryption keys to the cloud with an extra layer
of security. We're happy to announce that the feature has now reached its end of
beta period.

The following API methods have changed:

- `POST api/v2/lockers` is now `POST api/v2/import_token`

- `GET api/v2/lockers` is now `GET api/v2/import_token`

- `GET api/v2/lockers/{id}` is no longer supported

You can now create
[import tokens](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens)
to enable added security for keys that you upload to
{{site.data.keyword.keymanagementserviceshort}}.

To find out more about your options for importing keys, check out
[Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).
For a guided tutorial, see
[Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).
{: tip}

## July 2019
{: #jul-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for private endpoints
{: #added-private-endpoints}

New as of: 2019-07-31

You can now connect to {{site.data.keyword.keymanagementserviceshort}} over the
{{site.data.keyword.cloud_notm}} private network by targeting a private endpoint
for the service.

To get started, enable
[virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external}
for your infrastructure account. For more information, see
[Using private endpoints](/docs/key-protect?topic=key-protect-private-endpoints).

## June 2019
{: #june-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for {{site.data.keyword.at_full_notm}}
{: #added-at-log-support}

New as of: 2019-06-22

You can now monitor API calls to the
{{site.data.keyword.keymanagementserviceshort}} service by using
{{site.data.keyword.at_full_notm}}.

To learn more about monitoring {{site.data.keyword.keymanagementserviceshort}}
activity, see
[{{site.data.keyword.at_full_notm}} events](/docs/key-protect?topic=key-protect-at-events).

## May 2019
{: #may-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} upgrades HSMs to FIPS 140-2 Level 3
{: #upgraded-hsms}

New as of: 2019-05-22

{{site.data.keyword.keymanagementserviceshort}} now uses
{{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 for cryptographic
storage and operations. Your {{site.data.keyword.keymanagementserviceshort}}
keys are stored in FIPS 140-2 Level 3-compliant, tamper-evident hardware for all
regions.

To learn more about the features and benefits of
{{site.data.keyword.cloud_notm}} HSM 7.0, check out the
[product page](/cloud/hardware-security-module){: external}.

### End of support: Cloud Foundry-based {{site.data.keyword.keymanagementserviceshort}} instances
{: #legacy-service-eol}

New as of: 2019-05-15

The legacy {{site.data.keyword.keymanagementserviceshort}} service, based on
Cloud Foundry, reached its end of support on 15 May 2019. Cloud Foundry-managed
{{site.data.keyword.keymanagementserviceshort}} instances are no longer
supported and updates to the legacy service will no longer be provided.
Customers are encouraged to use {{site.data.keyword.keymanagementserviceshort}}
instances that are IAM-managed to benefit from the latest features for the
service.

If you created your {{site.data.keyword.keymanagementserviceshort}}
instance after 15 December 2017, your instance is IAM-managed and it is
not affected by this change.

Need to remove a {{site.data.keyword.keymanagementserviceshort}} service
instance from the **Cloud Foundry Services** section of your
{{site.data.keyword.cloud_notm}} resource list? You can reach out to us in the
[Support Center](/unifiedsupport/cases/add){: external}
by submitting a request to remove the entry from your console view.
{: tip}

## March 2019
{: #mar-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for policy-based key rotation
{: #added-support-policy-key-rotation}

New as of: 2019-03-22

You can now use {{site.data.keyword.keymanagementserviceshort}} to associate a
rotation policy for your root keys.

For more information, see
[Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy).
To find out more about your key rotation options in
{{site.data.keyword.keymanagementserviceshort}}, check out
[Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Added: {{site.data.keyword.keymanagementserviceshort}} adds beta support for transport keys
{: #added-transport-keys-beta}

New as of: 2019-03-20

Enable the secure import of encryption keys to the cloud by creating transport
encryption keys for your {{site.data.keyword.keymanagementserviceshort}}
service.

For more information, see
[Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

Transport keys are currently a beta feature. Beta features can change at any
time, and future updates might introduce changes that are incompatible with the
latest version.
{: important}

## February 2019
{: #feb-2019}

### Changed: Legacy {{site.data.keyword.keymanagementserviceshort}} instances
{: #changed-legacy-service-instances}

New as of: 2019-02-13

{{site.data.keyword.keymanagementserviceshort}} instances that were
provisioned before 15 December 2017 are running on a legacy infrastructure that
is based on Cloud Foundry. This legacy
{{site.data.keyword.keymanagementserviceshort}} service will be decommissioned
on 15 May 2019.

#### What this means to you
{: #feb-2019-what-this-means}

If you have active production keys in an older
{{site.data.keyword.keymanagementserviceshort}} instance, ensure that
you migrate the keys to a new instance by 15 May 2019 to avoid losing
access to your encrypted data. You can check to see whether you're using a
legacy instance by navigating to your resource list from the
[{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

If your {{site.data.keyword.keymanagementserviceshort}} instance is
listed in the **Cloud Foundry Services** section of the
{{site.data.keyword.cloud_notm}} resource list, or if you're using a
`bluemix.net` API endpoint to target operations for
the service, you're using a legacy instance of the
{{site.data.keyword.keymanagementserviceshort}}. After 15 May 2019, the legacy
endpoint will no longer be accessible, and you won't be able to target the
service for operations.

Need help with migrating your encryption keys into a new
{{site.data.keyword.keymanagementserviceshort}} instance? For detailed steps,
check out the
[migration client in GitHub](https://github.com/IBM-Cloud/kms-migration-client){: external}.
{: tip}

## December 2018
{: #dec-2018}

### Changed: {{site.data.keyword.keymanagementserviceshort}} API endpoints
{: #changed-api-endpoints}

New as of: 2018-12-19

To align with {{site.data.keyword.cloud_notm}}'s new unified experience,
{{site.data.keyword.keymanagementserviceshort}} has updated the base URLs for
its service APIs.

You can now update your applications to reference the new `cloud.ibm.com`
endpoints.

- `keyprotect.us-south.bluemix.net` is now `us-south.kms.cloud.ibm.com`
- `keyprotect.us-east.bluemix.net` is now `us-east.kms.cloud.ibm.com`
- `keyprotect.eu-gb.bluemix.net` is now `eu-gb.kms.cloud.ibm.com`
- `keyprotect.eu-de.bluemix.net` is now `eu-de.kms.cloud.ibm.com`
- `keyprotect.au-syd.bluemix.net` is now `au-syd.kms.cloud.ibm.com`
- `keyprotect.jp-tok.bluemix.net` is now `jp-tok.kms.cloud.ibm.com`

Both URLs for each regional service endpoint are supported at this time.

## October 2018
{: #oct-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the Tokyo region
{: #added-tokyo-region}

New as of: 2018-10-31

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in
the Tokyo region.

For more information, see
[Regions and locations](/docs/key-protect?topic=key-protect-regions).

### Added: {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
{: #added-cli-plugin}

New as of: 2018-10-02

You can now use the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
to manage keys in your {{site.data.keyword.keymanagementserviceshort}} service
instance.

To learn how to install the plug-in, see
[Setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli).
To find out more about the {{site.data.keyword.keymanagementserviceshort}} CLI,
[check out the CLI reference doc](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).

## September 2018
{: #sept-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for on-demand key rotation
{: #added-key-rotation}

New as of: 2018-09-28

You can now use the {{site.data.keyword.keymanagementserviceshort}} to rotate
your root keys on-demand.

For more information, see
[Rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).

### Added: End to end security tutorial for your cloud app
{: #added-security-tutorial}

New as of: 2018-09-14

Looking for code samples to help you encrypt storage bucket content with your
own encryption keys?

You can now practice adding end to end security for your cloud application by
following
[the new tutorial](/docs/solution-tutorials?topic=solution-tutorials-cloud-e2e-security){: external}.

For more information, see
[check out the sample app in GitHub](https://github.com/IBM-Cloud/secure-file-storage){: external}.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the Washington DC region
{: #added-wdc-region}

New as of: 2018-09-10

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in
the Washington DC region.

For more information, see
[Regions and locations](/docs/key-protect?topic=key-protect-regions).

## August 2018
{: #aug-2018}

### Changed: {{site.data.keyword.keymanagementserviceshort}} API documentation URL
{: #changed-api-doc-url}

New as of: 2018-08-28

The {{site.data.keyword.keymanagementserviceshort}} API Reference has moved!

You can now access the API documentation at
[/apidocs/key-protect](/apidocs/key-protect){: external}.

## March 2018
{: #mar-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the Frankfurt region
{: #added-frankfurt-region}

New as of: 2018-03-21

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in
the Frankfurt region.

For more information, see
[Regions and locations](/docs/key-protect?topic=key-protect-regions).

## January 2018
{: #jan-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into Sydney region
{: #added-sydney-region}

New as of: 2018-01-31

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in
the Sydney region.

For more information, see
[Regions and locations](/docs/key-protect?topic=key-protect-regions).

## December 2017
{: #dec-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Bring Your Own Key (BYOK)
{: #added-byok-support}

New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} now supports Bring Your Own Key
(BYOK) and customer-managed encryption.

- Introduced
    [root keys](/docs/key-protect?topic=key-protect-envelope-encryption#key-types),
    also called Customer Root Keys (CRKs), as primary resources in the service.

- Enabled
    [envelope encryption](/docs/key-protect?topic=key-protect-integrate-cos#kp-cos-how)
    for {{site.data.keyword.cos_full_notm}} buckets.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the London region
{: #added-london-region}

New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} is now available in the London
region.

For more information, see
[Regions and locations](/docs/key-protect?topic=key-protect-regions).

### Changed: {{site.data.keyword.iamshort}} roles
{: #changed-iam-roles}

New as of: 2017-12-15

{{site.data.keyword.iamshort}} roles, which determine the actions that you can
perform on {{site.data.keyword.keymanagementserviceshort}} resources, have
changed.

- `Administrator` is now `Manager`
- `Editor` is now `Writer`
- `Viewer` is now `Reader`

For more information, see
[Managing user access](/docs/key-protect?topic=key-protect-manage-access).

## September 2017
{: #sept-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Cloud IAM
{: #added-iam-support}

New as of: 2017-09-19

You can now use {{site.data.keyword.iamshort}} to set and manage access policies
for your {{site.data.keyword.keymanagementserviceshort}} resources.

For more information, see
[Managing user access](/docs/key-protect?topic=key-protect-manage-access).


