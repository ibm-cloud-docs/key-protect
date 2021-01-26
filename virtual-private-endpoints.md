---

copyright:
  years: 2020
lastupdated: "2020-01-26"

keywords: instance settings, service settings, network access policies

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

After you set up your Virtual Private Cloud (VPC), you can send 
requests to the {{site.data.keyword.keymanagementservicelong}} service 
through a virtual private endpoint.
{: shortdesc}

The {{site.data.keyword.keymanagementserviceshort}} service
can be accessed through a Virtual Private Endpoint (VPE) from your 
[Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).

Virtual Private Endpoint (VPE) for VPC enables you to connect to 
{{site.data.keyword.keymanagementserviceshort}} from your VPC network 
by using the IP addresses of your choice, which are allocated from a 
subnet within your VPC. VPEs are bound to a 
[VPE gateway](/docs/vpc?topic=vpc-about-vpe) and serve as an intermediary 
that enables your virtual server instance to interact with 
{{site.data.keyword.keymanagementserviceshort}}.

To connect to {{site.data.keyword.keymanagementserviceshort}} by using a virtual private
endpoint, you must use the
{{site.data.keyword.keymanagementserviceshort}} API, SDK, or Terraform.
This capability is not available from the
{{site.data.keyword.keymanagementserviceshort}} UI.
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
  [access](/docs/vpc?topic=vpc-accessing-vpe-after-setup){: external}.
- Understand how to [view details](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway){: external} of 
  a Virtual Private Endpoint.


## Virtual Private Service Endpoints
{: #vpe-endpoints}

The following table lists regions where {{site.data.keyword.keymanagementserviceshort}} service supports VPE. 
It also lists {{site.data.keyword.keymanagementserviceshort}} endpoints supported from each region. You can 
connect to {{site.data.keyword.keymanagementserviceshort}} service in another region using supported endpoints. 
For example, from the Sydney region, you can use {{site.data.keyword.keymanagementserviceshort}} service in 
`us-south` region using the us-south endpoint.

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
| BNPP (EU_FR2)    | | | |
| | `private.eu-fr2.kms.cloud.ibm.com` | `crn:v1:bluemix:public:kms:eu-fr2:::endpoint:private.eu-fr2.kms.cloud.ibm.com` |
{: caption="Table 1. Lists private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}

