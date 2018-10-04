---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-04"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# What's new
{: #releases}

Stay up-to-date with the new features that are available for {{site.data.keyword.keymanagementservicefull}}. 

## October 2018
{: #oct-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
New as of: 2018-10-02

You can now use the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in to manage keys in your {{site.data.keyword.keymanagementserviceshort}} service instance.

To learn how to install the plug-in, see [Setting up the CLI](/docs/services/key-protect/set-up-cli.md). To find out more about the {{site.data.keyword.keymanagementserviceshort}} CLI, [check out the CLI reference doc](/docs/services/key-protect/cli-reference.md).

## September 2018
{: #sept-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for on-demand key rotation
New as of: 2018-09-28

You can now use the {{site.data.keyword.keymanagementserviceshort}} to rotate your root keys on-demand.

For more information, see [Rotating keys](/docs/services/key-protect/rotate-keys.md).

### Added: End to end security tutorial for your cloud app
New as of: 2018-09-14

Looking for code samples to help you encrypt storage bucket content with your own encryption keys?

You can now practice adding end to end security for your cloud application by following [the new tutorial](/docs/tutorials/cloud-e2e-security.html).

For more information, [check out the sample app in GitHub ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into US East region
New as of: 2018-09-10

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the US East region. 

For more information, see [Regions and locations](/docs/services/key-protect/regions.html).

## August 2018
{: #aug-2018}

### Changed: {{site.data.keyword.keymanagementserviceshort}} API documentation URL
New as of: 2018-08-28

The {{site.data.keyword.keymanagementserviceshort}} API Reference has moved! 

You can now access the API documentation at
https://console.bluemix.net/apidocs/key-protect. 

## March 2018
{: #mar-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into Frankfurt region
New as of: 2018-03-21

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Frankfurt region. 

For more information, see [Regions and locations](/docs/services/key-protect/regions.html).

## January 2018
{: #jan-2018}

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into Sydney region
New as of: 2018-01-31

You can now create {{site.data.keyword.keymanagementserviceshort}} resources in the Sydney region. 

For more information, see [Regions and locations](/docs/services/key-protect/regions.html).

## December 2017
{: #dec-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Bring Your Own Key (BYOK)
New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} now supports Bring Your Own Key (BYOK) and customer-managed encryption.

- Introduced [root keys](/docs/services/key-protect/concepts/envelope-encryption.html#key-types), also called Customer Root Keys (CRKs), as primary resources in the service. 
- Enabled [envelope encryption](/docs/services/key-protect/integrations/integrate-cos.html#kp-cos-how) for {{site.data.keyword.cos_full_notm}} buckets.

### Added: {{site.data.keyword.keymanagementserviceshort}} expands into London region
New as of: 2017-12-15

{{site.data.keyword.keymanagementserviceshort}} is now available in the London region. 

For more information, see [Regions and locations](/docs/services/key-protect/regions.html).

### Changed: {{site.data.keyword.iamshort}} roles
New as of: 2017-12-15

{{site.data.keyword.iamshort}} roles, which determine the actions that you can perform on {{site.data.keyword.keymanagementserviceshort}} resources, have changed.

- `Administrator` is now `Manager`
- `Editor` is now `Writer`
- `Viewer` is now `Reader`

For more information, see [Managing user access](/docs/services/key-protect/manage-access.html).

## September 2017
{: #sept-2017}

### Added: {{site.data.keyword.keymanagementserviceshort}} adds support for Cloud IAM
New as of: 2017-09-19

You can now use {{site.data.keyword.iamshort}} to set and manage access policies for your {{site.data.keyword.keymanagementserviceshort}} resources.

For more information, see [Managing user access](/docs/services/key-protect/manage-access.html).