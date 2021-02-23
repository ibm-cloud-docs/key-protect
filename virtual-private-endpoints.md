---

copyright:
  years: 2020
lastupdated: "2020-02-08"

keywords: instance settings, service settings, network access policies, virtual private endpoints, private gateway, VPE

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

# Using a virtual private endpoint
{: #virtual-private-endpoints}

After you created your VPC and you want to connect to 
{{site.data.keyword.keymanagementserviceshort}} service for your 
data encryption needs, you can create a virtual private endpoint 
in your VPC to access Key Protect service within your VPC network.
{: shortdesc}

You can configure the VPE to use the IP addresses of 
your choice, which are allocated from a subnet within your VPC. 
VPEs are bound to a [VPE gateway](/docs/vpc?topic=vpc-about-vpe) 
and serve as an intermediary 
that enables your workload to interact with 
{{site.data.keyword.keymanagementserviceshort}}.

To connect to {{site.data.keyword.keymanagementserviceshort}} by using a 
virtual private endpoint, you must use the 
{{site.data.keyword.keymanagementserviceshort}} API, SDK, or Terraform.
The {{site.data.keyword.keymanagementserviceshort}} UI has to be accessed 
through public network from your VPC.
{: note}

## Before you begin
{: #private-network-prereqs}

Before you target a virtual private endpoint for
{{site.data.keyword.keymanagementserviceshort}}:

- Ensure that you have [provisioned a Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started){: external}.
- Ensure that you have conducted [planning for Virtual Private Endpoints](/docs/vpc?topic=vpc-planning-considerations){: external}.
- Ensure that [correct access controls](/docs/vpc?topic=vpc-vpe-configuring-acls){:external} 
  are set for your virtual private endpoint.
- Understand the [limitations](/docs/vpc?topic=vpc-limitations-vpe){: external} of having a virtual private endpoint.
- Ensure that you have [created](/docs/vpc?topic=vpc-ordering-endpoint-gateway){: external} and understand how to 
  [access](/docs/vpc?topic=vpc-accessing-vpe-after-setup){: external} a VPE gateway.
- Understand how to [view details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway){: external} of 
  a Virtual Private Endpoint.


## Virtual Private Service Endpoints
{: #vpe-endpoints}

The following table lists regions where {{site.data.keyword.keymanagementserviceshort}} service supports VPE. 
It also lists {{site.data.keyword.keymanagementserviceshort}} endpoints supported from each region. You can 
connect to {{site.data.keyword.keymanagementserviceshort}} service in another region using supported endpoints. 
For example, from the Sydney region, you can use {{site.data.keyword.keymanagementserviceshort}} service in 
`us-south` region using the us-south endpoint.

When connecting to a VPE via [CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway#vpe-ordering-cli) 
or [API](/docs/vpc?topic=vpc-ordering-endpoint-gateway#vpe-ordering-api), you will 
need to specify the CRN of the region that you will use to connect to the 
{{site.data.keyword.keymanagementserviceshort}} service. Use the table below to locate the CRN 
of the target region.
{: note}

| Region  | Endpoints Supported in Region | CRN |                                                                                                                                                                                                            
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| Dallas  | | |
| |`private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
| Washington  | | | |
| |`private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
| Sydney    | | | |
| | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
| Tokyo    | | | |
| | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
| London    | | | |
| | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
| Frankfurt    | | | |
| | `private.us-south.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:us-south:::endpoint:private.us-south.kms.cloud.ibm.com` | 
| |`private.us-east.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:us-east:::endpoint:private.us-east.kms.cloud.ibm.com`| 
| |`private.eu-gb.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-gb:::endpoint:private.eu-gb.kms.cloud.ibm.com`|
| |`private.eu-de.kms.cloud.ibm.com` |`crn:v1:bluemix:public:kms:eu-de:::endpoint:private.eu-de.kms.cloud.ibm.com`|
| |`private.au-syd.kms.cloud.ibm.com`|`crn:v1:bluemix:public:kms:au-syd:::endpoint:private.au-syd.kms.cloud.ibm.com`|
| |`private.jp-tok.kms.cloud.ibm.com`| `crn:v1:bluemix:public:kms:jp-tok:::endpoint:private.jp-tok.kms.cloud.ibm.com`|
{: caption="Table 1. Lists private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
