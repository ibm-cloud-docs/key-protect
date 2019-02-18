---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}

# Regions and locations
{: #regions}

You can connect your applications with the {{site.data.keyword.keymanagementservicelong}} service by specifying a regional service endpoint.
{: shortdesc}

## Available regions
{: #available-regions}

{{site.data.keyword.keymanagementserviceshort}} is available in the following regions and locations:
![The image shows the regions where the Key Protect service is available.](images/world-map_min.svg)

## Service endpoints
{: #service-endpoints}

If you are managing your {{site.data.keyword.keymanagementserviceshort}} resources programmatically, see the following table to determine the API endpoints to use when you connect to the [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect): 

<table>
    <tr>
        <th>Location</th>
        <th>Service API endpoint</th>
    </tr>
    <tr>
        <td>Dallas</td>
        <td>
            <code>us-south.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Washington DC</td>
        <td>
            <code>us-east.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>London</td>
        <td>
            <code>eu-gb.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Frankfurt</td>
        <td>
            <code>eu-de.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Sydney</td>
        <td>
            <code>au-syd.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Tokyo</td>
        <td>
            <code>jp-tok.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Table 1. Shows the available endpoints for the {{site.data.keyword.keymanagementserviceshort}} API</caption>
</table>

You can continue to use `https://keyprotect.<region>.bluemix.net` to target the service for operations, or you can update your applications with the new `cloud.ibm.com` endpoints. 
{: tip}

For more information about authenticating with {{site.data.keyword.keymanagementserviceshort}}, see [Accessing the API](/docs/services/key-protect?topic=key-protect-set-up-api).