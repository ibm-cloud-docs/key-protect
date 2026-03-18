---

copyright:
  years: 2020, 2026
lastupdated: "2026-02-11"

keywords: network access policies, virtual private endpoints, VPE

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

# Accessing virtual private endpoints in specific regions
{: #virtual-private-endpoints}

After you create your VPC, you can create a virtual private endpoint (VPE) to connect to {{site.data.keyword.keymanagementserviceshort}} for your data encryption needs. The VPE allows you to access {{site.data.keyword.keymanagementserviceshort}} within your VPC network.
{: shortdesc}

You can configure the VPE to use IP addresses of your choice from a subnet within your VPC. VPEs bind to a [VPE gateway](/docs/vpc?topic=vpc-about-vpe) and serve as an intermediary that enables your workload to interact with {{site.data.keyword.keymanagementserviceshort}}.

## Before you begin
{: #vpe-prereqs}

Before you target a VPE for
{{site.data.keyword.keymanagementserviceshort}}:

- Make sure that you have [provisioned a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started){: external}.
- Make sure that you have conducted [planning for Virtual Private Endpoints](/docs/vpc?topic=vpc-planning-considerations){: external}.
- Make sure that you set the [correct access controls](/docs/vpc?topic=vpc-using-acls){: external} for your VPE.
- Understand the [limitations](/docs/vpc?topic=vpc-limitations-vpe){: external} of having a VPE.
- Make sure that you [created](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external} a VPE gateway and understand how to [access](/docs/vpc?topic=vpc-accessing-vpe-after-setup){: external} it.
- Understand how to [view details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway){: external} of 
    a VPE.

You might need to manually update VPE settings, specifically the Internet Protocol (IP) address, during [Disaster recovery and business continuity actions](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).
{: important}

## Virtual Private Service Endpoints
{: #vpe-endpoints}

The following table lists regions where {{site.data.keyword.keymanagementserviceshort}} supports VPE. It also lists the {{site.data.keyword.keymanagementserviceshort}} endpoints that are supported from each region. You can connect to {{site.data.keyword.keymanagementserviceshort}} in another region by using supported endpoints. For example, from the Sydney region, you can use {{site.data.keyword.keymanagementserviceshort}} in the `us-south` region by using the us-south endpoint.

When you connect to a VPE by using the [CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=cli) or [API](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=api), specify the CRN of the region that you use to connect to {{site.data.keyword.keymanagementserviceshort}}. Use the following table to locate the CRN of the target region.
{: note}

| Region     | Endpoints Supported in Region        | CRN                                                                                |
|------------|--------------------------------------|------------------------------------------------------------------------------------|
| Dallas     |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
| Washington |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
| Sydney     |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
| Tokyo      |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
| London     |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
| Frankfurt  |                                      |                                                                                    |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |
{: caption="Table 1. VPE endpoints and CRNs for {{site.data.keyword.keymanagementserviceshort}}" caption-side="bottom"}
