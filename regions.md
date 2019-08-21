---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-31"

keywords: Key Protect API endpoints, available regions

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
{:deprecated: .deprecated}

# Regions and endpoints
{: #regions}

Review region and connectivity options for interacting with {{site.data.keyword.keymanagementservicelong}}.
{: shortdesc}

## Available regions
{: #available-regions}

{{site.data.keyword.keymanagementserviceshort}} is available in the following regions:

![The image shows the regions where the Key Protect service is available.](images/world-map_min.svg){: caption="Figure 1. Displays the regions where you can create and manage {{site.data.keyword.keymanagementserviceshort}} resources." caption-side="bottom"}

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one of the supported {{site.data.keyword.cloud_notm}} regions, which represent the geographic area where your {{site.data.keyword.keymanagementserviceshort}} requests are handled and processed. To learn more, see [Locations, tenancy, and availability](/docs/services/key-protect?topic=key-protect-ha-dr#availability).

## Connectivity options
{: #connectivity-options}

{{site.data.keyword.keymanagementserviceshort}} offers two connectivity options for interacting with its service APIs.

<dl>
    <dt>Public endpoints</dt>
        <dd>By default, you can connect to resources in your account over the {{site.data.keyword.cloud_notm}} public network. Your data is encrypted in transit by using the Transport Security Layer (TLS) 1.2 protocol.
        </dd>
    <dt>Private endpoints</dt>
        <dd>For added benefits, you can also enable <a href="/docs/account?topic=account-vrf-service-endpoint" target="_blank" class="external"> virtual routing and forwarding (VRF) and service endpoints</a> for your infrastructure account. When you enable VRF for your account, you can connect to {{site.data.keyword.keymanagementserviceshort}} by using a private IP that is accessible only through the {{site.data.keyword.cloud_notm}} private network. To learn more about VRF, see <a href="/docs/resources?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud" target="_blank" class="external">Virtual routing and forwarding on {{site.data.keyword.cloud_notm}}</a>. To learn how to connect to {{site.data.keyword.keymanagementserviceshort}} by using a private endpoint, see <a href="/docs/services/key-protect?topic=key-protect-private-endpoints">Using private endpoints</a>.
        </dd>
</dl>

## Service endpoints
{: #service-endpoints}

If you are managing your {{site.data.keyword.keymanagementserviceshort}} resources programmatically, see the following table to determine the API endpoints to use when you connect to the [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect). 

| Region        | Public endpoints             |
| ------------- | ---------------------------- |
| Dallas        | `us-south.kms.cloud.ibm.com` |
| Washington DC | `us-east.kms.cloud.ibm.com`  |
| London        | `eu-gb.kms.cloud.ibm.com`    |
| Frankfurt     | `eu-de.kms.cloud.ibm.com`    |
| Sydney        | `au-syd.kms.cloud.ibm.com`   |
| Tokyo         | `jp-tok.kms.cloud.ibm.com`   |
{: caption="Table 1. Lists public endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's public network" caption-side="top"}
{: #table-1}
{: tab-title="Public"}
{: class="comparison-tab-table"}
{: row-headers}

| Region        | Private endpoints                    |
| ------------- | ------------------------------------ |
| Dallas        | `private.us-south.kms.cloud.ibm.com` |
| Washington DC | `private.us-east.kms.cloud.ibm.com`  |
| London        | `private.eu-gb.kms.cloud.ibm.com`    |
| Frankfurt     | `private.eu-de.kms.cloud.ibm.com`    |
| Sydney        | `private.au-syd.kms.cloud.ibm.com`   |
| Tokyo         | `private.jp-tok.kms.cloud.ibm.com`   |
{: caption="Table 2. Lists private endpoints for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs over IBM Cloud's private network" caption-side="top"}
{: #table-2}
{: tab-title="Private"}
{: class="comparison-tab-table"}
{: row-headers}

For more information about authenticating with {{site.data.keyword.keymanagementserviceshort}}, see [Accessing the API](/docs/services/key-protect?topic=key-protect-set-up-api).