---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# Setting up the API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} provides a REST API that can be used with any programming language to store, retrieve, and generate encryption keys.
{: shortdesc}

## Retrieving your {{site.data.keyword.cloud_notm}} credentials
{: #retrieve-credentials}

To work with the API, you need to generate your service and authentication credentials. 

To gather your credentials:

1. [Generate an {{site.data.keyword.cloud_notm}} IAM access token](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Retrieve the instance ID that uniquely identifies your {{site.data.keyword.keymanagementserviceshort}} service instance](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## Forming your API request
{: #form-api-request}

When you make an API call to the service, structure your API request according to how you initially provisioned your instance of {{site.data.keyword.keymanagementserviceshort}}. 

To build your request, pair a [regional service endpoint](/docs/services/key-protect?topic=key-protect-regions) with the appropriate authentication credentials. For example, if you created a service instance for the `us-south` region, use the following endpoint and API headers to browse keys in your service:

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

Replace `<access_token>` and `<instance_ID>` with your retrieved service and authentication credentials.

Want to track your API requests in case something goes wrong? When you include the `-v` flag as part of cURL request, you get a `correlation-id` value in the response headers. You can use this value to correlate and track the request for debugging purposes.
{: tip} 

## What's next
{: #set-up-api-next-steps}

You're all set to start managing your encryption keys in Key Protect. To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](https://{DomainName}/apidocs/key-protect){: external}.
