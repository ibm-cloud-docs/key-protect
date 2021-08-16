---

copyright:
  years: 2017, 2019
lastupdated: "2020-08-22"

keywords: Key Protect integration, KMS provider

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

# Registering your service as an {{site.data.keyword.cloud_notm}} KMS provider
{: #kms-providers}

{{site.data.keyword.keymanagementservicelong}} is a public, multi-tenant key
management service (KMS) that provides KMS functionality for
{{site.data.keyword.cloud_notm}}. As support for dedicated and private KMS
environments becomes available, Bring Your Own Key (BYOK) adopting teams can
integrate their services directly to {{site.data.keyword.cloud_notm}} KMS
providers to offer tailored KMS experiences for cloud customers.
{: shortdesc}

## Before you begin
{: #kms-providers-before-you-begin}

Only follow the steps outlined here, if and only if your service has been
designated as a KMS provider.

- You should have a basic understanding of
[Global Search and Tagging (GhoST)](/docs/get-coding?topic=get-coding-ghost_overview){: external} service

- You should have a basic understanding of your service broker and how
[resource controller](/docs/get-coding?topic=get-coding-getting-started-with-the-resource-controller-onboarding-overview){: external}
provisions it into the
[resource catalog](https://globalcatalog.cloud.ibm.com){: external}

## List of {{site.data.keyword.cloud_notm}} KMS providers
{: #kms-providers-list}

Teams can integrate to the following list of KMS providers:

| KMS provider | Environment type |
| ------------ | ---------------- |
| {{site.data.keyword.keymanagementserviceshort}} | Public, multi-tenant KMS with a cloud-based HSM |
| [{{site.data.keyword.hscrypto}}](/docs/hs-crypto?topic=hs-crypto-overview){: external} | Dedicated KMS with cloud-based HSM |
| {{site.data.keyword.keymanagementserviceshort}} for [{{site.data.keyword.cloud_notm}} Private](https://ibm.com/support/knowledgecenter/SSBS6K/product_welcome_cloud_private.html){: external} | Private KMS with on-premises HSM |

All KMS providers use the
[{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}.
{: note}

To integrate with a KMS provider, adopting teams must modify how their service
discovers KMS instances for a given user so that the corresponding KMS endpoints
are found dynamically, without needing to hardcode to the
{{site.data.keyword.keymanagementserviceshort}} public API.

In turn, KMS providers need to register their services appropriately and provide
additional information to the resource controller to make themselves and their
instances easily discoverable by the adopting teams.

## Setting up your service as a KMS provider
{: #set-up-kms-provider}

To designate a service as a KMS provider, teams can modify the resource catalog
entry for their offering so that the resource controller identifies the service
by its `sub_type` field.

With this change, teams that want to integrate with a KMS provider can search
for all KMS instances that belong to a given user by specifying the `sub_type`
value in the
[Global Search and Tagging (GhoST)](/docs/get-coding?topic=get-coding-ghost_overview){: external}
service.

### Step 1: Updating the catalog entry
{: #update-catalog}

You can use the
[{{site.data.keyword.cloud_notm}} resource catalog API](/apidocs/globalcatalog){: external}
to update the catalog entry for your offering, and then designate your service
as a KMS provider.

To set up your service as a KMS provider:

1. In the [resource catalog](https://globalcatalog.cloud.ibm.com){: external},
    search the name of your offering to inspect its catalog entry.

2. From the _Overview_ tab, note the ID value that uniquely identifies your
    offering in the resource catalog service.

3. Use the catalog ID to retrieve the JSON representation of the catalog entry
    for your offering.

    ```sh
    $ curl -s "https://globalcatalog.cloud.ibm.com/api/v1/<catalog_entry_ID>?complete=true" > catalog_entry.json
    ```
    {: codeblock}

    Replace `<catalog_entry_ID>` with the value that you retrieved in the
    previous step. Then, open the `catalog_entry.json` file to inspect the
    details about your offering.

4. In `catalog_entry.json` file, search for the `metadata.service.type` field,
    and then specify `kms` as its value.

5. Update the catalog entry for your offering by calling the resource catalog
    API.

    ```sh
    $ curl -X PUT \
        "https://globalcatalog.cloud.ibm.com/api/v1/<catalog_entry_ID>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "content-type: application/json" \
        -d @catalog_entry.json
    ```
    {: codeblock}

    Replace `<catalog_entry_ID>` with the value that you retrieved in step 2.

### Step 2: Updating your broker
{: #update-broker}

Update your broker so that it returns the following details to the
[resource controller](/docs/get-coding?topic=get-coding-getting-started-with-the-resource-controller-onboarding-overview){: external}
when it invokes your {{site.data.keyword.keymanagementserviceshort}} instance
creation API.

```json
{
    "dashboard_url": "<your dashboard url>",
    "extensions": {
        "endpoints": {
            "public": "<public endpoint for the instance>",
            "private": "<optional - private network endpoint for the instance>"
        }
    }
}
```
{: codeblock}

This information is passed by the resource controller to
[Global Search and Tagging (GhoST)](/docs/get-coding?topic=get-coding-ghost_overview){: external}
for indexing as part of the instance entry metadata.

## Next Steps
{: #provider_next_step}

Now, that you're a KMS provider, run a quick test.

1. Provision an instance of your service

2. Manually discover your instance through the
    [platform API response](/docs/key-protect?topic=key-protect-onboard-service#discover-kms-instances)

3. Examine the response body has both your private and public endpoints

4. Use the endpoint(s) to create a root key


