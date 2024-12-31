---

copyright:
  years: 2017, 2024
lastupdated: "2024-12-31"

keywords: key protect, release notes, service updates

subcollection: key-protect

content-type: release-note

---

{:shortdesc: .shortdesc}
{:release-note: data-hd-content-type='release-note'}
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

# Release notes for {{site.data.keyword.keymanagementserviceshort}}
{: #key-protect-relnotes}

Stay up-to-date with the new features that are available for {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

## January 2025
{: #key-protect-jan25}

### 01 January 2025
{: #key-protect-jan0125}

{{site.data.keyword.keymanagementserviceshort}} announces the availability of a new Cross-region resliency pricing plan. Users who want the enhanced cross regional resliency of this plan can either create a new instance using this plan or switch existing instances to this plan starting on 03 January 2025. For more information, check out [Pricing for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}](/docs/key-protect?topic=key-protect-pricing-plan).

Additionally, the pricing plan formerly known as "Key version pricing" has been renamed "Standard", and does not have enhanced cross regional resiliency. Also, the five free key versions per account has been discontinued.



## July 2024
{: #key-protect-jul24}

### 01 July 2024
{: #key-protectjul0124}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces the availability of:

* The ability to manage [key management interoperability protocol (KMIP) adapters](/docs/key-protect?topic=key-protect-kmip) using [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_kmip_adapters)
* The ability to list kmip_adapters associated with a specific crk_id [using the API](/apidocs/key-protect#get-kmip-adapters), [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-kmip-adapter-list), and SDK.
* The ability to force delete a kmip_object [using the API](/apidocs/key-protect#delete-kmip-object).

## April 2024
{: #key-protect-apr24}

### 23 April 2024
{: #key-protect-apr2324}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces native support in the UI, CLI, SDK, and API for [key management interoperability protocol (KMIP) adapters](/docs/key-protect?topic=key-protect-kmip), an alternative to the [KMIP for VMWare](/docs/vmwaresolutions?topic=vmwaresolutions-kmip-overview) solution offering on {{site.data.keyword.cloud_notm}}.

## February 2024
{: #key-protect-feb24}

### 13 February 2024
{: #key-protect-feb1324}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces that adding a [key description](/docs/key-protect?topic=key-protect-create-root-keys) is now available as an option in the control plane (UI).

{{site.data.keyword.keymanagementserviceshort}} also announces the ability to access private endpoints using the {{site.data.keyword.keymanagementserviceshort}} control plane UI, allowing users to create and manage keys for instances using a private endpoint (for example, in a Satellite location). Similarly, keys created using the CLI or the SDK or related method can now be seen and updated using the UI.

## December 2023
{: #key-protect-dec23}

### 15 December 2023
{: #key-protect-dec1523}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces that {{site.data.keyword.keymanagementservicefull}} on Satellite is now available in the  {{site.data.keyword.cloud_notm}} `eu-de` Multi-Zone Region (MZR). Previously, only the `us-east` region was available. 

For more information about {{site.data.keyword.keymanagementservicefull}} on Satellite, check out [About {{site.data.keyword.keymanagementservicefull}} on Satellite](/docs/key-protect?topic=key-protect-satellite-about).

## November 2023
{: #key-protect-nov23}

### 9 November 2023
{: #key-protect-nov923}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces the availability of:

* The ability to add a description to a key [using Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key#description).
* The ability to force delete a key ring [using the API](/apidocs/key-protect#deletekeyring), [Terraform](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key_rings#force_delete), and the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-ring-delete).

## September 2023
{: #key-protect-sep23}

### 15 September 2023
{: #key-protect-sep1523}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces that it is now live in the Madrid MZR. For more information about the regions in which {{site.data.keyword.keymanagementserviceshort}} is available, check out [Regions and endpoints](/docs/key-protect?topic=key-protect-regions).

### 12 September 2023
{: #key-protect-sep1223}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces the availability of the ability to add a description to a key [using the CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-create).

## August 2023
{: #key-protect-oct23}

### 30 August 2023
{: #key-protect-oct3023}
{: release-note}

Because {{site.data.keyword.keymanagementserviceshort}} does not allow instances that have keys in them to be deleted, it is required that any keys in an instance be deleted before the instance itself can be deleted. However, because of the "soft" deletion of keys, it is possible that a user might delete a key and then soon after delete an instance. This deletion of the instance also permanently deletes the key, even if the deletion of the key is recent enough to have made it eligible to be restored.

For this reason, {{site.data.keyword.keymanagementserviceshort}} now supports the reclamation of instances for a short time after they have been deleted. To see if your instance can be reclaimed, check out [Listing reclaimed resources by using the CLI](/docs/account?topic=account-resource-reclamation&interface=cli#list-reclaimed-cli). For the commands on reclaiming the resource (the instance in this case), check out [Restoring a resource by using the CLI](/docs/account?topic=account-resource-reclamation&interface=cli#restore-resource-cli).

For more information about restoring keys, check out [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys).

## June 2023
{: #key-protect-june23}

### 27 June 2023
{: #key-protect-june2723}
{: release-note}

The 0.8.0 version of the {{site.data.keyword.keymanagementserviceshort}} CLI is now available. This version includes support for counting the number of [key versions](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-versions) regardless of whether the key is in an active state. Also, this version of the CLI includes native support for Macs that use an [Apple Silicon](https://support.apple.com/en-us/116943) processor. Previously, users received a warning during the installation of the {{site.data.keyword.keymanagementserviceshort}} CLI plugin.

## May 2023
{: #key-protect-may23}

### 24 May 2023
{: #key-protect-may2423}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces a new integration with {{site.data.keyword.powerSys_notm}} for AIX and Linux.

The integration with AIX allows you to use {{site.data.keyword.powerSys_notm}} to leverage AIX file systems with the `keysvrmgr` and `hdcryptmgr` command. For more information, check out [Using Hyper Protect Crypto Services (HPCS) and Key Protect for AIX](/docs/power-iaas?topic=power-iaas-integrate-hpcs#AIX-hpcs).

The {{site.data.keyword.powerSys_notm}} for Linux integration prevents Linux Unified Key Setup (LUKS) encryption keys from being compromised using {{site.data.keyword.keymanagementserviceshort}} as a single point of control to enable or disable access to data across the enterprise. This is done by successively wrapping encryption keys, with the ultimate control being a master key that resides in a hardware security module (HSM). For more information, check out [Using Hyper Protect Crypto Services (HPCS) or Key Protect for Linux](/docs/power-iaas?topic=power-iaas-integrate-hpcs#Linux-hpcs) and [Protect LUKS encryption keys with IBM Cloud Hyper Protect Crypto Services and Key Protect](https://developer.ibm.com/tutorials/protect-luks-encryption-keys-with-ibm-cloud-hyper-protect-crypto-services/).

## April 2023
{: #key-protect-apr23}

### 14 April 2023
{: #key-protect-apr1423}
{: release-note}

The {{site.data.keyword.keymanagementservicefull}} on Satellite [Pricing plan](/docs/key-protect?topic=key-protect-pricing-plan-satellite) is now active. Unlike the [pricing for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}](/docs/key-protect?topic=key-protect-pricing-plan), the pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite is not based on key versions. Instead, reflecting the different use cases and infrastructure maintenance requirements for Satellite-based deployments, users are charged a flat rate for each location where they want {{site.data.keyword.keymanagementserviceshort}} installed and then an additional rate depending on the quota of keys they select at deployment time.

For more information about {{site.data.keyword.keymanagementservicefull}} on Satellite, check out [About {{site.data.keyword.keymanagementservicefull}} on Satellite](/docs/key-protect?topic=key-protect-satellite-about).

## March 2023
{: #key-protect-mar23}

### 24 March 2023
{: #key-protect-mar2423}
{: release-note}

The price per key version in {{site.data.keyword.cloud_notm}} has risen 4.5% in staging, an increase from USD1 to USD1.045. This price increases in production on April 1, 2023 and applies to all new and existing key versions. For more information, check out [Pricing](/docs/key-protect?topic=key-protect-pricing-plan).

## February 2023
{: #key-protect-feb23}

### 3 February 2023
{: #key-protect-feb323}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces ability to view number of key versions for keys in all states
:  Previously, the [List key versions API endpoint](/apidocs/key-protect#getkeyversions) would return an HTTP 409 if a key was not in an `Active` (1) state. When a new parameter, `allKeyStates`, is passed as true, it returns the number of versions of a key even if the key is no longer active. Similarly, an HTTP 410 used to be returned for keys in a `Destroyed` (5) state. Now, if `allKeyStates` is passed as true, the number of versions of the destroyed key is returned.

For more information, check out [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).

## December 2022
{: #key-protect-dec22}

### 2 December 2022
{: #key-protect-dec222}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new features for the Terraform plugin as part of v1.48.0
:  New features include support of instance policies (for example, to set a default key rotation period), the ability to create a key and override any instance rotation policies that might exist, and the ability to enable or disable a rotation policy on a key. Also, a known bug with the endpoint URL was fixed via the environment variable.

## October 2022
{: #key-protect-oct22}

### 14 October 2022
{: #key-protect-oct1422}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new features for the CLI Plugin
:   New features include support for creating instance rotation policies using the [UI](/docs/key-protect?topic=key-protect-set-rotation-policy) and the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-instance-policies) as well as the ability to enable and disable rotation policies for an individual key at key creation time using the [UI](/docs/key-protect?topic=key-protect-set-rotation-policy#manage-policies-gui) and the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-policies). Also, the latest release includes enhanced filtering for keys the [UI](/docs/key-protect?topic=key-protect-view-keys#view-keys-gui) and the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-keys).

### 7 October 2022
{: #key-protect-oct0722}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new features for the API
:   A [new `Create Key with Policy Overrides`](/apidocs/key-protect#createkeywithpoliciesoverrides) method and [`filter` parameter used in the `GET keys` method](/apidocs/key-protect#getkeys) highlight the ease of setting security policies on user-specified targets. In addition, you can now [set instance policies on multiple options](/apidocs/key-protect#putinstancepolicy), like authorizing dual deletion and rotation, with the ability to enable and disable specific policies.

## August 2022
{: #key-protect-aug22}

### 24 August 2022
{: #key-protect-aug2422}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces a new release of the CLI plugin
:   The new version supersedes the old version and upgrading your installation is recommended. New features include support for the new search parameter to filter the list of keys returned from the [`keys` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-keys) and support also for the new sort parameters to order the list of keys returned from the [`keys` command](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-keys).

## June 2022
{: #key-protect-jun22}

### 22 June 2022
{: #key-protect-jun2222}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new features
:   Added support for sorting a list of keys returned by the service based on one or more key properties. Sorting keys will first be available in the [API](/apidocs/key-protect). For more information, see [the parameter for sorting keys using the `GET /keys` method from the API](/apidocs/key-protect#getkeys).

## May 2022
{: #key-protect-may22}

### 25 May 2022
{: #key-protect-may2522}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new features
:   Added support for searching a list of keys returned by the service, limiting the number of keys returned. Searching keys will first be available in the [UI](/login) as well as from the [API](/apidocs/key-protect). For more information, see [Viewing keys in the console](/docs/key-protect?topic=key-protect-view-keys&interface=ui#view-keys-gui).

## March 2022
{: #key-protect-mar22}

### 08 March 2022
{: #key-protect-mar0822}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new enhancements
:   Added support for querying the total count of [versions of a key](/docs/key-protect?topic=key-protect-view-key-versions) using the [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect#getkeyversions). And as part of a continuing program of improvement to the [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect), [key aliases](/docs/key-protect?topic=key-protect-create-key-alias) are now supported as key identifiers in place of key IDs in addition to [Retrieving a key](/docs/key-protect?topic=key-protect-retrieve-key&interface=api#get-key-api). Multiple API methods, like `POST`, `PATCH`, and `DELETE`, and features like [key purge](/apidocs/key-protect#purgekey), and [key restore](/apidocs/key-protect#restorekey) now include this enhancement. As an enhancement to the [{{site.data.keyword.keymanagementserviceshort}} Key Ring API](/apidocs/key-protect#listkeyrings), optional query parameters have been added to the listing of key rings to support [pagination](/docs/key-protect?topic=key-protect-grouping-keys&interface=api#list-key-ring-api).

## December 2021
{: #key-protect-dec21}

### 15 December 2021
{: #key-protect-dec1521}
{: release-note}

{{site.data.keyword.keymanagementservicefull}} announces new {{site.data.keyword.satellitelong_notm}} support
:   {{site.data.keyword.keymanagementservicefull}} now supports [{{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-getting-started) where you use your own compute infrastructure that is in your on-premises data center, other cloud providers, or edge networks to more fully control your own encryption keys by creating your own instance of {{site.data.keyword.keymanagementserviceshort}}.

    - For more information, check out [About {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-about).

## September 2021
{: #key-protect-sep21}

### 15 September 2021
{: #key-protect-sep1521}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces new deprecations
:   This announcement begins the deprecation of creating policies using the [`ibm_kms_key` resource](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key){: external} used with {{site.data.keyword.terraform-provider_full}}. While migrating your code to use the new [Key Policies](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/kms_key_policies){: external}, please refrain from using the existing resource unless setting the [Lifecycle "ignore" policies block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key#lifecycle-ignore-block-example){: external}. As part of continued migration and improvement, the `algorithmBitSize`, `algorithmMode`, `algorithmType` and `algorithmMetadata` fields will no longer be operational within the Key Protect [API](/apidocs/key-protect){: external}.

### 08 September 2021
{: #key-protect-sep0821}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} support for {{site.data.keyword.terraform-provider_full}} enhancements
:   [{{site.data.keyword.terraform-provider_full}}](/docs/schematics) now supports creating and retrieving [Key Policies](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/kms_key_policies){: external} when creating and retrieving keys through {{site.data.keyword.terraform-provider_full_notm}} as a separate resource.

### 04 September 2021
{: #key-protect-sep0421}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} CLI plug-in v.0.6.5
:   The [release of {{site.data.keyword.keymanagementserviceshort}} CLI version 0.6.5](/docs/key-protect?topic=key-protect-cli-changelog) introduces new structures for empty results when querying. Learn more at [the CLI reference](/docs/key-protect?topic=key-protect-key-protect-cli-reference).

## August 2021
{: #key-protect-aug21}

### 05 August 2021
{: #key-protect-aug0521}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} support for key purge enhancements
:   The ability to purge keys that have been deleted after four hours using the UI has been added. For more information, check out [About purging and deleting keys](/docs/key-protect?topic=key-protect-delete-purge-keys#delete-purge-keys-considerations). Also, the ability to view your instance ID and cloud resource name (CRN) has been made easier. For more information, check out [Retrieving your instance ID and cloud resource name (CRN)](/docs/key-protect?topic=key-protect-retrieve-instance-ID).

## June 2021
{: #key-protect-jun21}

### 30 June 2021
{: #key-protect-jun3021}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} CLI plug-in v.0.6.3
:   The {{site.data.keyword.keymanagementserviceshort}} CLI plugin has been updated to version 0.6.3. Minor changes in this [release](/docs/key-protect?topic=key-protect-key-protect-cli-reference) include support for {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.hscrypto}} specific algorithms in [Key Create](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-create) and [Key Rotate](/docs/key-protect?topic=key-protect-key-protect-cli-reference#kp-key-rotate).

## April 2021
{: #key-protect-apr21}

### 22 April 2021
{: #key-protect-apr2221}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces changes to the console
:   Many of the panels and actions you can take in the console have been modified to accommodate the addition of key rings and add other functionality to the console that previously was only possible using the APIs. This includes a new **Key rings** panel where key rings can be managed and a **Key ring ID** column in the **Keys** panel.

   - Note that as part of this change, the **Instance policies** and **Manage keys** panels in the console have been renamed **Instance policies** and **Keys** respectively.
   - This release also adds the ability to use the API to purge keys four hours after they have been moved to the _Destroyed_ state. For more information, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).
   - The ability to purge keys using the UI was added in the [August, 2021 release](#key-protect-aug0521).

## March 2021
{: #key-protect-mar21}

### 12 March 2021
{: #key-protect-mar1221}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports key transfers in key rings
:   You can now use the {{site.data.keyword.keymanagementserviceshort}} API to transfer a key from one key ring to another key ring. In order to move a key, you must have _Manager_ IAM access permissions to both the key and the target key ring, which is the key ring that you would like the key to be transferred to. To find out more, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys#transfer-key-key-ring-api). Also, support for aliases and key-rings brings best practices to using the [{{site.data.keyword.keymanagementserviceshort}} CLI plug-in](/docs/key-protect?topic=key-protect-key-protect-cli-reference). Learn more about all of the new features in the [CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## February 2021
{: #key-protect-feb21}

### 25 February 2021
{: #key-protect-feb2521}
{: release-note}

The process to restore keys has been enhanced
:   Deleted keys can now be restored up to 30 days after deletion. After 30 days, it is no longer possible to restore a key. Any and all types of keys (standard or root, imported or created) can be restored. It is no longer required to pass in key material when restoring a key. For more information, check out [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys). Rotating keys and re-wrapping encrypted content with a new DEK is fundamental to security. Quickly retrieve keys having extractable content with [this search feature](/docs/key-protect?topic=key-protect-view-keys#filter-keys-extractable-state-api).

### 15 February 2021
{: #key-protect-feb1521}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces UI enhancements
:   Managing keys through the UI interface has been enhanced with new options presented in a simple and convenient selectable option menu within the context of each managed key. Simply click on the "overflow" icon (`⋯`) at the end of each row to access common features and UI enhancements.

   - Note that "Associated Resources" are now accessible within the context of each key in the console, as well as having its own item in the menu. Also, setting a key's rotation policy is now possible in the same panel where a key can be manually rotated.
   - Also, the list of endpoints in the console has grown with the addition of Osaka. You can now update your applications to reference the new endpoint.

{{site.data.keyword.keymanagementserviceshort}} supports additional UI features
:   Now, users can [Wrap](/docs/key-protect?topic=key-protect-wrap-keys) and [Unwrap](/docs/key-protect?topic=key-protect-unwrap-keys) active root keys to provide envelope encryption. For more information, see Protecting data with [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption) for an overview.

### 15 March 2021
{: #key-protect-mar1521}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports key purge
:   Beginning in April 2021, {{site.data.keyword.keymanagementserviceshort}} will implement a key purge feature that will automatically purge any keys that have been deleted for more than 90 days. A purged key and its associated data will be permanently removed, or hard deleted, from the {{site.data.keyword.keymanagementserviceshort}} database. When a user or service deletes a key in Key Protect today, if the key is not restored within 30 days, the key is soft deleted. All key data, except key material data, remains in {{site.data.keyword.keymanagementserviceshort}}, and those details are retrievable by List Keys, Get Key and other APIs. Once automatic key purge is introduced, users will not be able to retrieve any information regarding a purged key. Any API calls that use the Key ID of a purged key will result in a 404 HTTP Not Found error.

   - Note: A key purge can be reversed by restoring the hard deleted key.
   - **How will the changes impact my environment?** The majority of users will not notice an impact. Please note that any data related to a purged key (key metadata, registrations, policies, etc) will no longer be available via the {{site.data.keyword.keymanagementserviceshort}} service. If you are required to retain any data related to a purged key (key metadata, registrations, policies, etc) for an extended period of time, it is recommended to perform the necessary API or CLI calls to retrieve and store that data in your own storage device.

### 05 February 2021
{: #key-protect-feb0521}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports new UI actions
:   If you have _Manager_ access permissions, you can filter for keys in the **Destroyed** state and restore an imported root key via the ⋯ icon on the **Keys** table. You can use the restore key side panel to complete the process for restoring the key. For more information, see [Restoring a deleted key with the console](/docs/key-protect?topic=key-protect-restore-keys#restore-ui).

{{site.data.keyword.keymanagementserviceshort}} supports private networks
:   You can now connect to {{site.data.keyword.keymanagementserviceshort}} from your virtual private cloud (VPC) via a virtual private endpoint (VPE). VPEs are bound to a [VPE gateway](/docs/vpc?topic=vpc-about-vpe) and serve as an intermediary that enables your workload to interact with {{site.data.keyword.keymanagementserviceshort}}.

   - To get started, [provision a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started){: external} and create a [VPE gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external}. For more information, see [Using private endpoints](/docs/key-protect?topic=key-protect-private-endpoints).

## January 2021
{: #key-protect-jan21}

### 27 January 2021
{: #key-protect-jan2721}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports key rings
:   You can now use the {{site.data.keyword.keymanagementserviceshort}} REST API to manage access to a specific set of keys that are bundle a collection called a `key ring`. You can manage and restrict access to key rings to via IAM policies. To find out more, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).

### 06 January 2021
{: #key-protect-jan0621}
{: release-note}

Restore key process is now improved
:   If you have _Manager_ access permissions, you can now use the the {{site.data.keyword.keymanagementserviceshort}} UI to restore all unexpired-deleted keys. For more information, see [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys#restore-ui). You can now use the {{site.data.keyword.keymanagementserviceshort}} REST API to initiate a manual data synchronization request to to synchronize your service's key records with what is in {{site.data.keyword.keymanagementserviceshort}}'s database records. For more information, see [Sync associated resources](/docs/key-protect?topic=key-protect-sync-associated-resources){: external}.

   This API is available only for users if a cloud service has enabled the key registiation feature as part of its integration with {{site.data.keyword.keymanagementserviceshort}}. To learn if an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) supports key registration, refer to its service documentation for more information. 
   {: preview}

## December 2020
{: #key-protect-dec20}

### 11 December 2020
{: #key-protect-dec1120}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports operational metrics
:   If you have _Manager_ IAM access permissions, you can now use the {{site.data.keyword.keymanagementserviceshort}} UI to create a metrics policy that allows you to view the operational metrics for your {{site.data.keyword.keymanagementserviceshort}} instance. To find out more, see [Managing metrics](/docs/key-protect?topic=key-protect-manage-monitor-metrics#enable-metrics-instance-policy-ui).

### 01 December 2020
{: #key-protect-dec0120}
{: release-note}

Announcing support for key aliases
:   If you have _Writer_ or _Manager_ access permissions, you can now use the {{site.data.keyword.keymanagementserviceshort}} REST API to create a key alias. You can use a key alias to refer to a key in your {{site.data.keyword.keymanagementserviceshort}} service instance. To find out more, see [Creating key aliases](/docs/key-protect?topic=key-protect-create-key-alias).

## October 2020
{: #key-protect-oct20}

### 20 October 2020
{: #key-protect-oct2020}
{: release-note}

Announcing Quantum Safe Cryptography
:   In preparation for the post-quantum era, you can use a quantum safe enabled TLS connection to secure your communication to the {{site.data.keyword.keymanagementservicefull}} service. To find out more, see [Using Quantum Safe Cryptography](/docs/key-protect?topic=key-protect-quantum-safe-cryptography-tls-introduction).

### 12 October 2020
{: #key-protect-oct1220}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds access policy UI support
:   You can set a key creation and importation policy, in the [user interface (UI)](/login/){: external}, to restrict how keys are created and imported into your {{site.data.keyword.keymanagementserviceshort}} service instance. See the updated "Instance policies" pane in the {{site.data.keyword.keymanagementserviceshort}} UI.

## September 2020
{: #key-protect-sep20}

### 17 September 2020
{: #key-protect-sep1720}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds access policy API support
:   You can set a key creation and importation policy, using the [API](/apidocs/key-protect){: external}, to restrict how keys are created and imported into your {{site.data.keyword.keymanagementserviceshort}} service instance. To find out more, see [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess).

### 22 September 2020
{: #key-protect-sep2220}
{: release-note}

Updates to the {{site.data.keyword.keymanagementserviceshort}} UI
:   The {{site.data.keyword.keymanagementserviceshort}} UI now has support for the following feature:

   - List keys by key state: You can now use the {{site.data.keyword.keymanagementserviceshort}} UI to filter and retrieve keys that are in a specified state. For more information, see [Viewing keys in the console](/docs/key-protect?topic=key-protect-view-keys#filter-key-state-gui).
   - If you have _Manager_ access permissions, you can filter for keys in the **Destroyed** state and restore an imported root key via the ⋯ icon on the **Keys** table. Note: you must include the original Key Material to restore the key.

### 09 September 2020
{: #key-protect-sep0920}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} CLI plug-in v.0.5.2
:   The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in version 0.5.2
was updated with these changes:

   - Commands that specify JSON outout (`--output json`) now return an empty JSON structure if there is no output. The [CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog) has all CLI updates.

## July 2020
{: #key-protect-jul20}

### 21 July 2020
{: #key-protect-jul2120}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} CLI plug-in v.0.5.1
:   The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is used to manage keys in your instance. To install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see [setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli). For a detailed explanation of changes in version 0.5.1, see the [CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## June 2020
{: #key-protect-jun20}

### 24 June 2020
{: #key-protect-jun2420}
{: release-note}

Updates to the {{site.data.keyword.keymanagementserviceshort}} UI
:   The {{site.data.keyword.keymanagementserviceshort}} UI now has support for the following features:

   - Enable/disable key: If you have _Manager_ access permissions, you can now use the {{site.data.keyword.keymanagementserviceshort}} UI to suspend or restore a key's encrypt and decrypt operations. For more information, see [Disabling root keys](/docs/key-protect?topic=key-protect-disable-keys#disable-enable-ui)
   - Restore key: If you have _Manager_ access permissions, you can now use the the {{site.data.keyword.keymanagementserviceshort}} UI to restore a previously imported root key that was deleted. For more information, see [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys#restore-ui).
   - Set an instance level dual authorization policy: You can now use the {{site.data.keyword.keymanagementserviceshort}} UI to require two users to safely delete a key from your {{site.data.keyword.keymanagementserviceshort}} instance. For more information, see [Enabling a dual authorization policy for an instance](/docs/key-protect?topic=key-protect-manage-dual-auth#enable-dual-auth-instance-policy-ui).
   - Set an instance level network policy: You can now use the {{site.data.keyword.keymanagementserviceshort}} UI to restrict requests to public or private networks. For more information, see [Managing Network Access Policies](/docs/key-protect?topic=key-protect-managing-network-access-policies#enabling-network-access-to-your-service-instance-ui).

### 19 June 2020
{: #key-protect-jun1920}
{: release-note}

Announcing {{site.data.keyword.keymanagementserviceshort}} CLI plug-in v.0.5.0
:   The {{site.data.keyword.keymanagementserviceshort}} CLI plug-in is used to manage keys in your instance. To install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in, see [setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli). For a detailed explanation of changes in version 0.5.0, see the [CLI changelog](/docs/key-protect?topic=key-protect-cli-changelog).

## May 2020
{: #key-protect-may20}

### 29 May 2020
{: #key-protect-may2920}
{: release-note}

Announcing new {{site.data.keyword.at_full_notm}} event field support
:   Beginning in late May 2020, {{site.data.keyword.keymanagementserviceshort}} will return updated event fields in [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external} logs. These updates will be available across all supported regions by 29 May 2020.

   - Successful replace registration, update registration, and unwrap key events
    will change from severity level `warning` to `normal`.
   - The `rewrapedKeyVersionId` field will change to `rewrappedKeyVersionId`.
   - The `TotalResources` field will change to `totalResources`.
   - **Why are we making these changes?** These changes are required to remove deprecated event fields and support upcoming service enhancements for {{site.data.keyword.at_full_notm}}.
   - **How will the changes impact my environment?** This change impacts the event fields that are returned in [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external} audit logs when you perform {{site.data.keyword.keymanagementserviceshort}} actions. The change does not impact {{site.data.keyword.keymanagementserviceshort}} operations. As a security or compliance admin, ensure that the removed and changed event fields do not affect your audit operations.

### 01 May 2020
{: #key-protect-may0120}
{: release-note}

Announcing new permissions for existing roles
:   If you have _Writer_ or _Manager_ access permissions, you can now use the {{site.data.keyword.keymanagementserviceshort}} REST API to rotate an root key that was initially imported with an import token. To find out more, see [Using an import token to rotate a key](/docs/key-protect?topic=key-protect-rotate-keys#rotate-keys-secure-api). If you have _Manager_ access permissions, you can now use the the {{site.data.keyword.keymanagementserviceshort}} REST API to restore a previously imported root key. To find out more, see [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys). If you have _Manager_ access permissions, you can now use the the {{site.data.keyword.keymanagementserviceshort}} REST API to suspend or restore a keys encrypt and decrypt operations. To find out more, see [Disabling keys](/docs/key-protect?topic=key-protect-disable-keys).

## April 2020
{: #key-protect-apr20}

### 16 April 2020
{: #key-protect-apr1620}
{: release-note}

Announcing network access policies
:   You can set a network access policy to allow API requests to a {{site.data.keyword.keymanagementserviceshort}} instance from public or private networks. To find out more, see [Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies).

## March 2020
{: #key-protect-mar20}


### 14 March 2020
{: #key-protect-mar1420}
{: release-note}

Announcing support for key metadata and key versions
:   If you have _Reader_ access permissions, you can now use the {{site.data.keyword.keymanagementserviceshort}} REST API to view only details about a specific standard key without retrieving the key itself. To find out more, see [Viewing details about a key](/docs/key-protect?topic=key-protect-retrieve-key-metadata). You can now audit the rotation history of a root key by viewing its key versions. After you rotate a root key, the ID of the root key does not change, but {{site.data.keyword.keymanagementserviceshort}} now returns key version information to help you determine which version of the root key is protecting your data. To find out more, see [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).

## February 2020
{: #key-protect-feb20}

### 28 February 2020
{: #key-protect-feb2820}
{: release-note}

Beginning in April 2020, {{site.data.keyword.keymanagementserviceshort}} will return updated event fields
:    These updates will be available in [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external} logs across all supported regions by 15 April 2020. This change impacts the following {{site.data.keyword.at_full_notm}} event fields. Affected event fields include removed event fields, such as `meta`, `observer.typeURI`, `requestHeader`, `requestPath`, `responseBody`, `type`, and `typeURI`. The `eventTime` field will change from format `2020-02-03T20:20:37+0000` to `2020-02-03T20:20:37Z`. The `target.name` field is currently set to {{site.data.keyword.keymanagementserviceshort}}. This value will change to the name of the resource on which the operation was performed. For example, the name of the encryption key, or the name of your {{site.data.keyword.keymanagementserviceshort}} instance. New event fields include `requestData` and `responseData`.

   - **Why are we making these changes?** These changes are required to remove deprecated event fields and support upcoming service enhancements for {{site.data.keyword.at_full_notm}}.
   - **How will the changes impact my environment?** This change impacts the event fields that are returned in [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-monitor_events){: external} audit logs when you perform {{site.data.keyword.keymanagementserviceshort}} actions. The change does not impact {{site.data.keyword.keymanagementserviceshort}} operations. As a security or compliance admin, ensure that the removed and changed event fields do not affect your audit operations.

### 25 February 2020
{: #key-protect-feb2520}
{: release-note}

Support for integrated services and resources
:   You can now use {{site.data.keyword.keymanagementserviceshort}} REST APIs to examine which root keys are actively protecting what data so that you can evaluate exposures based on your organization's security or compliance needs. For more information, see [View associations between root keys and {{site.data.keyword.cloud_notm}} resources](/docs/key-protect?topic=key-protect-view-protected-resources). This extra feature is available only if a cloud service has enabled it as part of its integration with {{site.data.keyword.keymanagementserviceshort}}. To learn if an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) supports key registration, refer to its service documentation for more information. Also, {{site.data.keyword.keymanagementserviceshort}} enabled extra security measures to protect against the accidental or malicious deletion of keys.

   - {{site.data.keyword.keymanagementserviceshort}} now blocks the deletion of a root key that's actively protecting a cloud resource. To learn if a key is registered to cloud resource, you can [review the resources](/docs/key-protect?topic=key-protect-view-protected-resources) that are associated with the key.
   -  You can now [force deletion on a key](/docs/key-protect?topic=key-protect-delete-keys#delete-keys-force-delete) that's protecting a cloud resource.

### 17 February 2020
{: #key-protect-feb1720}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces additional roles
:   Need to grant read-only access to keys? You can now choose between the _Reader_ and _ReaderPlus_ IAM roles for better control over access to key material. To learn more about service access roles, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

## January 2020
{: #key-protect-jan20}

### 15 January 2020
{: #key-protect-jan1520}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} announces new dual authorization policies
:   You can now enable dual authorization policies to safely delete keys from your {{site.data.keyword.keymanagementserviceshort}} instance. When you enable dual authorization, you require an action from two users to delete a key.

   - To learn how to enable dual authorization, see [Using dual authorization policies for the deletion of keys](/docs/key-protect?topic=key-protect-manage-dual-auth).

## November 2019
{: #key-protect-nov19}

### 04 November 2019
{: #key-protect-110419}
{: release-note}

Added:
:   {{site.data.keyword.keymanagementserviceshort}} is updating its user access roles and how they correspond to {{site.data.keyword.keymanagementserviceshort}} service actions. Effective 13 November 2019, {{site.data.keyword.keymanagementserviceshort}} will update access roles accordingly:

   - Create keys service action has current role assignments: Administrator, Editor, Writer, Manager; will have new Writer, Manager role or roles.
   - Retrieve a key by ID service action has current role assignments: Administrator, Editor, Writer, Manager; will have new Writer, Manager role or roles.
   - Retrieve a list of keys service action has current role assignments: Administrator, Editor, Writer, Manager, Viewer, Reader; will have new Reader, Writer, Manager role or roles.
   - Wrap keys service action has current role assignments: Administrator, Editor, Writer, Manager, Viewer, Reader; will have new Reader, Writer, Manager role or roles.
   - Unwrap keys service action has current role assignments: Administrator, Editor, Writer, Manager, Viewer, Reader; will have new Reader, Writer, Manager role or roles.
   - Rewrap keys service action has current role assignments: Administrator, Editor, Writer, Manager, Viewer, Reader; will have new Reader, Writer, Manager role or roles.
   - Rotate keys service action has current role assignments: Administrator, Editor, Writer, Manager; will have new Writer, Manager role or roles.
   - Set rotation policies service action has current role assignments: Administrator, Manager; will have new Manager role or roles.
   - Retrieve rotation policies service action has current role assignments: Administrator, Manager; will have new Manager role or roles.
   - Delete a key by ID service action has current role assignments: Administrator, Manager; will have new Manager role or roles.

   As an account owner or admin, review the existing access policies for all {{site.data.keyword.keymanagementserviceshort}} users in your account to ensure that they are assigned the appropriate levels of access. To learn more about {{site.data.keyword.keymanagementserviceshort}} roles and permissions, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).
   {: note}

## September 2019
{: #key-protect-sept19}

### 27 September 2019
{: #key-protect-sep2719}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} supports fine-grain access
:   As an account admin, you can now assign fine-grained access to individual keys within a {{site.data.keyword.keymanagementserviceshort}} instance. To learn more about granting access, see [Granting access to keys](/docs/key-protect?topic=key-protect-grant-access-keys).

### 16 September 2019
{: #key-protect-sep1619}
{: release-note}

Transport keys deprecated, replaced with import tokens
:   On 20 March 2019, [{{site.data.keyword.keymanagementserviceshort}} announced transport keys](#key-protect-mar2019) as a beta feature for importing encryption keys to the cloud with an extra layer of security. We're happy to announce that the feature has now reached its end of
beta period. The following API methods have changed:

   - `POST api/v2/lockers` is now `POST api/v2/import_token`
   - `GET api/v2/lockers` is now `GET api/v2/import_token`
   - `GET api/v2/lockers/{id}` is no longer supported

   You can now create [import tokens](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens) to enable added security for keys that you upload to {{site.data.keyword.keymanagementserviceshort}}.
   {: note}

   To find out more about your options for importing keys, check out
   [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).
   For a guided tutorial, see
   [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).
   {: tip}

## July 2019
{: #key-protect-jul19}

### 31 July 2019
{: #key-protect-jul3119}
{: release-note}

Announcing private endpoint support
:   You can now connect to {{site.data.keyword.keymanagementserviceshort}} over the {{site.data.keyword.cloud_notm}} private network by targeting a private endpoint for the service.

   - To get started, enable [virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external} for your infrastructure account. For more information, see [Using private endpoints](/docs/key-protect?topic=key-protect-private-endpoints).

## June 2019
{: #key-protect-jun19}

### 22 June 2019
{: #key-protect-jun2219}
{: release-note}

Announcing {{site.data.keyword.at_full_notm}} integration
:   You can now monitor API calls to the {{site.data.keyword.keymanagementserviceshort}} service by using {{site.data.keyword.at_full_notm}}. To learn more about monitoring {{site.data.keyword.keymanagementserviceshort}} activity, see [{{site.data.keyword.at_full_notm}} events](/docs/key-protect?topic=key-protect-at-events).

## May 2019
{: #key-protect-may19}

### 22 May 2019
{: #key-protect-052219}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} now uses {{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 for cryptographic storage and operations
:   Your {{site.data.keyword.keymanagementserviceshort}} keys are stored in FIPS 140-2 Level 3-compliant, tamper-evident hardware for all regions. To learn more about the features and benefits of {{site.data.keyword.cloud_notm}} HSM 7.0, check out the [product page](/cloud/hardware-security-module){: external}.

### 15 May 2019
{: #key-protect-051519}
{: release-note}

The legacy {{site.data.keyword.keymanagementserviceshort}} service, based on
Cloud Foundry, reached its end of support on 15 May 2019.
:   Cloud Foundry-managed {{site.data.keyword.keymanagementserviceshort}} instances are no longer supported and updates to the legacy service will no longer be provided. Customers are encouraged to use {{site.data.keyword.keymanagementserviceshort}} instances that are IAM-managed to benefit from the latest features for the service. If you created your {{site.data.keyword.keymanagementserviceshort}} instance after 15 December 2017, your instance is IAM-managed and it is not affected by this change.

   - Need to remove a {{site.data.keyword.keymanagementserviceshort}} service instance from the **Cloud Foundry Services** section of your {{site.data.keyword.cloud_notm}} resource list? You can reach out to us in the [Support Center](/unifiedsupport/cases/add){: external} by submitting a request to remove the entry from your console view.

## March 2019
{: #key-protect-mar19}

### 22 March 2019
{: #key-protect-mar2219}
{: release-note}

Announcing rotation policies for root keys
:   You can now use {{site.data.keyword.keymanagementserviceshort}} to associate a rotation policy for your root keys. For more information, see [Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy). To find out more about your key rotation options in {{site.data.keyword.keymanagementserviceshort}}, check out [Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### 20 March 2019
{: #key-protect-mar2019}
{: release-note}

Announcing secure import of encryption keys
:   Enable the secure import of encryption keys to the cloud by creating transport encryption keys for your {{site.data.keyword.keymanagementserviceshort}} service. For more information, see [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

## February 2019
{: #key-protect-feb19}
### 13 February 2019
{: #key-protect-feb1319}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} deprecates Cloud Foundry instances:
:   {{site.data.keyword.keymanagementserviceshort}} instances that were provisioned before 15 December 2017 are running on a legacy infrastructure that is based on Cloud Foundry. This legacy {{site.data.keyword.keymanagementserviceshort}} service will be decommissioned on 15 May 2019. If you have active production keys in an older {{site.data.keyword.keymanagementserviceshort}} instance, ensure that you migrate the keys to a new instance by 15 May 2019 to avoid losing access to your encrypted data. You can check to see whether you're using a legacy instance by navigating to your resource list from the [{{site.data.keyword.cloud_notm}} console](/login/){: external}. If your {{site.data.keyword.keymanagementserviceshort}} instance is listed in the **Cloud Foundry Services** section of the {{site.data.keyword.cloud_notm}} resource list, or if you're using a `bluemix.net` API endpoint to target operations for the service, you're using a legacy instance of the {{site.data.keyword.keymanagementserviceshort}}. After 15 May 2019, the legacy endpoint will no longer be accessible, and you won't be able to target the service for operations.

   Need help with migrating your encryption keys into a new
   {{site.data.keyword.keymanagementserviceshort}} instance? For detailed steps,
   check out the
   [migration client in GitHub](https://github.com/IBM-Cloud/kms-migration-client){: external}.
   {: tip}

## December 2018
{: #key-protect-dec18}

### 19 December 2018
{: #key-protect-dec1918}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} has updated endpoints:
:   To align with {{site.data.keyword.cloud_notm}}'s new unified experience, {{site.data.keyword.keymanagementserviceshort}} has updated the base URLs for its service APIs. You can now update your applications to reference the new `cloud.ibm.com` endpoints.

   - `keyprotect.us-south.bluemix.net` is now `us-south.kms.cloud.ibm.com`
   - `keyprotect.us-east.bluemix.net` is now `us-east.kms.cloud.ibm.com`
   - `keyprotect.eu-gb.bluemix.net` is now `eu-gb.kms.cloud.ibm.com`
   - `keyprotect.eu-de.bluemix.net` is now `eu-de.kms.cloud.ibm.com`
   - `keyprotect.au-syd.bluemix.net` is now `au-syd.kms.cloud.ibm.com`
   - `keyprotect.jp-tok.bluemix.net` is now `jp-tok.kms.cloud.ibm.com`

   Both URLs for each regional service endpoint are supported at this time.
   {: tip}

## October 2018
{: #key-protect-oct18}

### 31 October 2018
{: #key-protect-oct3118}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new regional support
:   You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Tokyo region. For more information, see [Regions and locations](/docs/key-protect?topic=key-protect-regions).

### 02 October 2018
{: #key-protect-oct0218}
{: release-note}

Announcing the new {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
:   You can now use the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to manage keys in your {{site.data.keyword.keymanagementserviceshort}} service instance. To learn how to install the plug-in, see [Setting up the CLI](/docs/key-protect?topic=key-protect-set-up-cli). To find out more about the {{site.data.keyword.keymanagementserviceshort}} CLI, [check out the CLI reference doc](/docs/key-protect?topic=key-protect-key-protect-cli-reference).

## September 2018
{: #key-protect-sept18}

### 28 September 2018
{: #key-protect-sep2818}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new key rotation feature
:   You can now use the {{site.data.keyword.keymanagementserviceshort}} to rotate your root keys on-demand. For more information, see [Rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).

### 14 September 2018
{: #key-protect-sep1418}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new sample application
:   Looking for code samples to help you encrypt storage bucket content with your own encryption keys? You can now practice adding end to end security for your cloud application by following [the new tutorial](/docs/solution-tutorials?topic=solution-tutorials-cloud-e2e-security){: external}. For more information, see [check out the sample app in GitHub](https://github.com/IBM-Cloud/secure-file-storage){: external}.

### 10 September 2018
{: #key-protect-sep1018}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new regional support
:   You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Washington DC region. For more information, see [Regions and locations](/docs/key-protect?topic=key-protect-regions).

## August 2018
{: #key-protect-aug18}

### 28 August 2018
{: #key-protect-aug2818}
{: release-note}

The {{site.data.keyword.keymanagementserviceshort}} API Reference has moved
:   You can now access the API documentation at [{{site.data.keyword.cloud_notm}} API Docs for {{site.data.keyword.keymanagementserviceshort}}](/apidocs/key-protect){: external}.

## March 2018
{: #key-protect-mar18}

### 21 March 2018
{: #key-protect-mar2118}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new regional support
:   You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Frankfurt region. For more information, see [Regions and locations](/docs/key-protect?topic=key-protect-regions).

## January 2018
{: #key-protect-jan18}

### 31 January 2018
{: #key-protect-jan3118}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} adds new regional support
:   You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Sydney region. For more information, see [Regions and locations](/docs/key-protect?topic=key-protect-regions).

## December 2017
{: #key-protect-dec17}

### 15 December 2017
{: #key-protect-dec1517}
{: release-note}

{{site.data.keyword.keymanagementserviceshort}} now supports Bring Your Own Key (BYOK) and customer-managed encryption
:   Introducing [root keys](/docs/key-protect?topic=key-protect-envelope-encryption#key-types), also called Customer Root Keys (CRKs), as primary resources in the service. This enables [envelope encryption](/docs/key-protect?topic=key-protect-integrate-cos#kp-cos-how) for {{site.data.keyword.cos_full_notm}} buckets. With this change, {{site.data.keyword.keymanagementserviceshort}} is now available in the London region. For more information, see [Regions and locations](/docs/key-protect?topic=key-protect-regions). Also, {{site.data.keyword.iamshort}} roles, which determine the actions that you can perform on {{site.data.keyword.keymanagementserviceshort}} resources, have changed.

   - `Administrator` is now `Manager`
   - `Editor` is now `Writer`
   - `Viewer` is now `Reader`

   For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).
   {: tip}

## September 2017
{: #key-protect-sept17}

### 19 September 2017
{: #key-protect-sep1917}
{: release-note}

Introducing Key Protect
:   {{site.data.keyword.keymanagementservicefull}} is a full-service encryption solution that allows data to be secured and stored in {{site.data.keyword.cloud_notm}} using the latest envelope encryption techniques that leverage FIPS 140-2 Level 3 certified cloud-based hardware security modules. You can use {{site.data.keyword.iamshort}} to set and manage access policies for your {{site.data.keyword.keymanagementserviceshort}} resources. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).
