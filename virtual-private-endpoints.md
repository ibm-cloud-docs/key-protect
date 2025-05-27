---

copyright:
  years: 2020, 2025
lastupdated: "2025-05-27"

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

After you created your VPC and you want to connect to 
{{site.data.keyword.keymanagementserviceshort}} service for your 
data encryption needs, you can create a virtual private endpoint (VPE)
in your VPC to access Key Protect service within your VPC network.
{: shortdesc}

You can configure the VPE to use the IP addresses of 
your choice, which are allocated from a subnet within your VPC. 
VPEs are bound to a [VPE gateway](/docs/vpc?topic=vpc-about-vpe) 
and serve as an intermediary 
that enables your workload to interact with 
{{site.data.keyword.keymanagementserviceshort}}.

## Before you begin
{: #vpe-prereqs}

Before you target a VPE for
{{site.data.keyword.keymanagementserviceshort}}:

- Ensure that you have [provisioned a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started){: external}.
- Ensure that you have conducted [planning for Virtual Private Endpoints](/docs/vpc?topic=vpc-planning-considerations){: external}.
- Ensure that [correct access controls](/docs/vpc?topic=vpc-using-acls){: external} 
    are set for your VPE.
- Understand the [limitations](/docs/vpc?topic=vpc-limitations-vpe){: external} of having a VPE.
- Ensure that you have [created](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external} and understand how to 
    [access](/docs/vpc?topic=vpc-accessing-vpe-after-setup){: external} a VPE gateway.
- Understand how to [view details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway){: external} of 
    a VPE.

VPE settings, specifically the Internet Protocol (IP) address, may need to be manually updated during [Disaster recovery and business continuity actions](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery). 
{: important}

## Virtual Private Service Endpoints
{: #vpe-endpoints}

The following table lists regions where {{site.data.keyword.keymanagementserviceshort}} service supports VPE. 
It also lists {{site.data.keyword.keymanagementserviceshort}} endpoints supported from each region. You can 
connect to {{site.data.keyword.keymanagementserviceshort}} service in another region using supported endpoints. 
For example, from the Sydney region, you can use {{site.data.keyword.keymanagementserviceshort}} service in 
`us-south` region using the us-south endpoint.

When connecting to a VPE via [CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=cli#vpe-ordering-cli&interface=cli) 
or [API](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=api#vpe-ordering-cli&interface=cli), you will 
need to specify the CRN of the region that you will use to connect to the 
{{site.data.keyword.keymanagementserviceshort}} service. Use the table below to locate the CRN 
of the target region.
{: note}

| Region     | Endpoints Supported in Region        | CRN                                                                                |  |
|------------|--------------------------------------|------------------------------------------------------------------------------------|--|
| Dallas     |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
| Washington |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
| Sydney     |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
| Tokyo      |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
| London     |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
| Frankfurt  |                                      |                                                                                    |  |
|            | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` |  |
|            | `private.us-east.kms.cloud.ibm.com`  | `crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`   |  |
|            | `private.eu-gb.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`       |  |
|            | `private.eu-de.kms.cloud.ibm.com`    | `crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`       |  |
|            | `private.au-syd.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`     |  |
|            | `private.jp-tok.kms.cloud.ibm.com`   | `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`     |  |
{: caption="Lists VPEs for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
