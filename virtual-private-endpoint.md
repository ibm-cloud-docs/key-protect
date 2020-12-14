---

copyright:
  years: 2020
lastupdated: "2020-10-08"

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
{: #private-endpoints}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you send requests to the {{site.data.keyword.keymanagementservicelong}} service 
through a virtual private endpoint.
{: shortdesc}

If you're using a Virtual Private Cloud (VPC) environment to run your service, you can connect
and use {{site.data.keyword.keymanagementserviceshort}} via a Virtual Private Endpoint (VPE). A
VPE extends your VPC's private network address so that you are able to leverage the 
{{site.data.keyword.keymanagementserviceshort}} service from a virtual machine.

To connect to {{site.data.keyword.keymanagementserviceshort}} by using a private
network connection, you must use the
{{site.data.keyword.keymanagementserviceshort}} API.
This capability is not available from the
{{site.data.keyword.keymanagementserviceshort}} GUI or CLI.
{: note}

## Before you begin
{: #private-network-prereqs}

Before you target a virtual private endpoint for
{{site.data.keyword.keymanagementserviceshort}}:

- Ensure that have [provisioned a Virtual Private Cloud](/vpc-ext/provision/vpc){: external}.

  For more information on configuring a VPC, see [Creating a VPC and subnet](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console#creating-a-vpc-and-subnet)
  {: important}



## Creating a virtual private endpoint via the UI console
{: #create-vpc}

To create a VPN gateway using the UI:

1. From your browser, open the [IBM Cloud console](https://cloud.ibm.com){: external}
   and log in to your account.
2. Select the Menu icon on the upper left of the page, then click `VPC Infrastructure > Virtual private endpoint gateway`
    in the Network section.
3. On the Virtual private endpoint gateways for VPC page, click `Create` and specify the following information:

        - Name - Enter a name for the VPN gateway, such as my-vpn-gateway.
        - Virtual Private Cloud - Select the VPC for the VPN gateway.
        - VPC Region - 
        - Resource group - Select a resource group for the VPN gateway.
        - Region - Shows the region where the VPC is located and where the VPN gateway will be provisioned.
        - Cloud Service Offerings - Select Key-Protect
        - Cloud Service Regions - You have the option to choose between Dallas(staging) or Sydney.
        - Endpoint - Choose which endpoint you would like to send traffic through using the selected service region.
4. Select a method of reserving an IP for your VPC gateway
5. Click the "Create virtual private endpoint gateway" button

To create a VPN gateway using the API or CLI, see [Creating a VPN gateway](/docs/vpc?topic=vpc-vpn-create-gateway).
{: note}

## Virtual Private Service Endpoints
{: #vpe-endpoints}

The following table lists all of the private endpoints that you can send traffic through using your VPC:

| Region        | Private endpoints                            |
| ------------- | -------------------------------------------- |
| Dallas        | `private.us-south.kms.cloud.ibm.com`         |
| Washington DC | `private.us-east.kms.cloud.ibm.com`          |
| London        | `private.eu-gb.kms.cloud.ibm.com`            |
| Frankfurt     | `private.eu-de.kms.cloud.ibm.com`            |
| Sydney        | `private.au-syd.kms.cloud.ibm.com`           |
| Tokyo         | `private.jp-tok.kms.cloud.ibm.com`           |
| Staging       | `private.qa.us-south.kms.test.cloud.ibm.com` |
{: caption="Table 1. Lists private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}

## Using an allowed ip policy with a Virtual Private Cloud
{: #vpe-allowed-ip}

When configuring your allowed ip policy for your {{site.data.keyword.keymanagementserviceshort}} instance, 
you'll need to add all of the ip addresses listed under the `Cloud Service Endpoint source addresses` to 
your allowed ip policy.

To find your VPC source addresses, follow these steps:

1. From your browser, open the [IBM Cloud console](https://cloud.ibm.com){: external}
   and log in to your account.
2. Select the Menu icon on the upper left of the page, then click `VPC Infrastructure > VPCs`
    in the Network section.
3. On the Virtual Private Clouds page, select the Virtual Private Cloud that will be 
   accessing your {{site.data.keyword.keymanagementserviceshort}} instance
4. View your `Cloud Service Endpoint source addresses` in the bottom right side of the
    VPC details page.

