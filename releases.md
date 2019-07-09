---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-02"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# What's new
{: #releases}

Stay up-to-date with the new features that are available for {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

## June 2019
{: #june-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for {{site.data.keyword.at_full_notm}}
{: #added-at-logdna-support}
New as of: 2019-06-22

You can now monitor API calls to the {{site.data.keyword.keymanagementserviceshort}} service by using {{site.data.keyword.at_full_notm}}. 

To learn more about monitoring {{site.data.keyword.keymanagementserviceshort}} activity, see [Activity Tracker events](/docs/services/key-protect?topic=key-protect-at-events).

## May 2019
{: #may-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} upgrades HSMs to FIPS 140-2 Level 3
{: #upgraded-hsms}
New as of: 2019-05-22

{{site.data.keyword.keymanagementserviceshort}} now uses {{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 for cryptographic storage and operations. Your {{site.data.keyword.keymanagementserviceshort}} keys are stored in FIPS 140-2 Level 3-compliant, tamper-evident hardware for all regions. 

To learn more about the features and benefits of {{site.data.keyword.cloud_notm}} HSM 7.0, check out the [product page](https://www.ibm.com/cloud/hardware-security-module){: external}.

### End of support: Cloud Foundry-based {{site.data.keyword.keymanagementserviceshort}} service instances
{: #legacy-service-eol}
New as of: 2019-05-15

The legacy {{site.data.keyword.keymanagementserviceshort}} service, based on Cloud Foundry, reached its end of support on 15 May 2019. Cloud Foundry-managed {{site.data.keyword.keymanagementserviceshort}} service instances are no longer supported and updates to the legacy service will no longer be provided. Customers are encouraged to use {{site.data.keyword.keymanagementserviceshort}} service instances that are IAM-managed to benefit from the latest features for the service.

If you created your {{site.data.keyword.keymanagementserviceshort}} service instance after 15 December 2017, your service instance is IAM-managed and it is not affected by this change. For additional questions, reach out to Terry Mosbaugh at [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).

Need to remove a {{site.data.keyword.keymanagementserviceshort}} service instance from the **Cloud Foundry Services** section of your {{site.data.keyword.cloud_notm}} resource list? You can reach out to us in the [Support Center](https://{DomainName}/unifiedsupport/cases/add) by submitting a request to remove the entry from your console view.
{: tip}

## March 2019
{: #mar-2019}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for policy-based key rotation
{: #added-support-policy-key-rotation}
New as of: 2019-03-22

You can now use {{site.data.keyword.keymanagementserviceshort}} to associate a rotation policy for your root keys.

For more information, see [Setting a rotation policy](/docs/services/key-protect?topic=key-protect-set-rotation-policy). To find out more about your key rotation options in {{site.data.keyword.keymanagementserviceshort}}, check out [Comparing your key rotation options](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Added: {{site.data.keyword.keymanagementserviceshort}} adds beta support for transport keys
New as of: 2019-03-20

Enable the secure import of encryption keys to the cloud by creating transport encryption keys for your {{site.data.keyword.keymanagementserviceshort}} service.

For more information, see [Bringing your encryption keys to the cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

Transport keys are currently a beta feature. Beta features can change at any time, and future updates might introduce changes that are incompatible with the latest version.
{: important}

## February 2019
{: #feb-2019}

### Changed: Legacy {{site.data.keyword.keymanagementserviceshort}} service instances
{: #changed-legacy-service-instances}

New as of: 2019-02-13

{{site.data.keyword.keymanagementserviceshort}} service instances that were provisioned before 15 December 2017 are running on a legacy infrastructure that is based on Cloud Foundry. This legacy {{site.data.keyword.keymanagementserviceshort}} service will be decommissioned on 15 May 2019.

**What this means to you**

If you have active production keys in an older {{site.data.keyword.keymanagementserviceshort}} service instance, ensure that you migrate the keys to a new service instance by 15 May 2019 to avoid losing access to your encrypted data. You can check to see whether you're using a legacy instance by navigating to your resource list from the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/). If your {{site.data.keyword.keymanagementserviceshort}} service instance is listed in the **Cloud Foundry Services** section of the {{site.data.keyword.cloud_notm}} resource list, or if you're using the  `https://ibm-key-protect.edge.bluemix.net` API endpoint to target operations for the service, you're using a legacy instance of the {{site.data.keyword.keymanagementserviceshort}}. After 15 May 2019, the legacy endpoint will no longer be accessible, and you won't be able to target the service for operations.

Need help with migrating your encryption keys into a new service instance? For detailed steps, check out the [migration client in GitHub](https://github.com/IBM-Cloud/kms-migration-client){: external}. If you have additional questions about the status of your keys or the migration process, reach out to Terry Mosbaugh at [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## December 2018
{: #dec-2018}

### Changed: {{site.data.keyword.keymanagementserviceshort}} API endpoints
{: #changed-api-endpoints}

New as of: 2018-12-19

To align with {{site.data.keyword.cloud_notm}}'s new unified experience, {{site.data.keyword.keymanagementserviceshort}} has updated the base URLs for its service APIs.

You can now update your applications to reference the new `cloud.ibm.com` endpoints.

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

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Tokyo region. 

For more information, see [Regions and locations](/docs/services/key-protect?topic=key-protect-regions).

### Added: {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
{: #added-cli-plugin}

New as of: 2018-10-02

You can now use the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to manage keys in your {{site.data.keyword.keymanagementserviceshort}} service instance.

To learn how to install the plug-in, see [Setting up the CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). To find out more about the {{site.data.keyword.keymanagementserviceshort}} CLI, [check out the CLI reference doc](/docs/services/key-protect?topic=key-protect-cli-reference).

## September 2018
{: #sept-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for on-demand key rotation
{: #added-key-rotation}

New as of: 2018-09-28

You can now use the {{site.data.keyword.keymanagementserviceshort}} to rotate your root keys on-demand.

For more information, see [Rotating keys](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Added: End to end security tutorial for your cloud app
{: #added-security-tutorial}

New as of: 2018-09-14

Looking for code samples to help you encrypt storage bucket content with your own encryption keys?

You can now practice adding end to end security for your cloud application by following [the new tutorial](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

For more information, [check out the sample app in GitHub](https://github.com/IBM-Cloud/secure-file-storage){: external}.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the Washington DC region
{: #added-wdc-region}

New as of: 2018-09-10

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Washington DC region. 

For more information, see [Regions and locations](/docs/services/key-protect?topic=key-protect-regions).

## August 2018
{: #aug-2018}

### Changed: {{site.data.keyword.keymanagementserviceshort}} API documentation URL
{: #changed-api-doc-url}

New as of: 2018-08-28

The {{site.data.keyword.keymanagementserviceshort}} API Reference has moved! 

You can now access the API documentation at
https://{DomainName}/apidocs/key-protect. 

## March 2018
{: #mar-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the Frankfurt region
{: #added-frankfurt-region}

New as of: 2018-03-21

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Frankfurt region. 

For more information, see [Regions and locations](/docs/services/key-protect?topic=key-protect-regions).

## January 2018
{: #jan-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into Sydney region
{: #added-sydney-region}

New as of: 2018-01-31

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Sydney region. 

For more information, see [Regions and locations](/docs/services/key-protect?topic=key-protect-regions).

## December 2017
{: #dec-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Bring Your Own Key (BYOK)
{: #added-byok-support}

New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} now supports Bring Your Own Key (BYOK) and customer-managed encryption.

- Introduced [root keys](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), also called Customer Root Keys (CRKs), as primary resources in the service. 
- Enabled [envelope encryption](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) for {{site.data.keyword.cos_full_notm}} buckets.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into the London region
{: #added-london-region}

New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} is now available in the London region. 

For more information, see [Regions and locations](/docs/services/key-protect?topic=key-protect-regions).

### Changed: {{site.data.keyword.iamshort}} roles
{: #changed-iam-roles}

New as of: 2017-12-15

{{site.data.keyword.iamshort}} roles, which determine the actions that you can perform on {{site.data.keyword.keymanagementserviceshort}} resources, have changed.

- `Administrator` is now `Manager`
- `Editor` is now `Writer`
- `Viewer` is now `Reader`

For more information, see [Managing user access](/docs/services/key-protect?topic=key-protect-manage-access).

## September 2017
{: #sept-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Cloud IAM
{: #added-iam-support}

New as of: 2017-09-19

You can now use {{site.data.keyword.iamshort}} to set and manage access policies for your {{site.data.keyword.keymanagementserviceshort}} resources.

For more information, see [Managing user access](/docs/services/key-protect?topic=key-protect-manage-access).