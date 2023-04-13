---

copyright:
  years: 2023
lastupdated: "2023-04-13"

keywords: pricing plan, cost, satellite

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
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}

# Pricing for Key Protect on Satellite
{: #pricing-plan-satellite}
{: ui}
{: api}
{: cli}

Unlike the pricing for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}, the pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite is not based on key versions. Instead, reflecting the different use cases and infrastructure maintenance requirements for Satellite-based deployments, users are charged a flat rate for each location where they want {{site.data.keyword.keymanagementserviceshort}} installed and then an additional rate depending on the quota of keys they select at deployment time.
{: shortdesc}

Because of the way {{site.data.keyword.keymanagementserviceshort}} structures its billing system, you cannot delete the first instance you deploy unless you plan to uninstall {{site.data.keyword.keymanagementserviceshort}} from your Satellite location completely.
{: tip}

## Details
{: #pricing-plan-satellite-details}

Pricing is for {{site.data.keyword.keymanagementserviceshort}} on Satellite only and does not include the price of a Satellite location or the price to install {{site.data.keyword.cloud_notm}} Database (ICD) creation in your location.
{: note}

Pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite is based on two metrics:

* **Base price**: a monthly base price charged for the installation of {{site.data.keyword.keymanagementserviceshort}} into a Satellite location. This is $780 a month per location. Note that there is no additional charge for creating multiple instances in a location.

* **Key quota**: the maximum number of [non-deleted keys](#pricing-plan-satellite-non-deleted) which is defined when {{site.data.keyword.keymanagementserviceshort}} is deployed in the Satellite location. This quota covers the number of keys per location, not per instance. If you choose to create a second instance, it is bound by the quota selected for the location. You do not need to increase your quota. Keys cost $1 per key, while quotas can be requested in batches of 100 keys. Because the minimum quota that can be selected is 100, the minimum quota charge is $100. If 200 keys are selected, for example, the charge is $200. This quota of keys can be changed later by opening a service ticket. If you attempt to exceed your key quota, you will get an error.

If you change quotas in the middle of a billing cycle (for example, going from 100 keys to 200 keys) you are charged whichever quota is larger. This charge is not prorated by the amount of time a particular quota was selected and is based on the maximum quota during a particular billing cycle.
{: important}

If you navigate to the [catalog page for {{site.data.keyword.keymanagementserviceshort}} on Satellite](https://cloud.ibm.com/catalog/services/key-protect), click on the **Satellite** card under **Select a location**, and cannot see the specific plan information for {{site.data.keyword.keymanagementserviceshort}} on Satellite, this might be because you have not been allowlisted. [Contact {{site.data.keyword.IBM_notm}}](https://www.ibm.com/contact/us/en/) to learn more.

## What is a non-deleted key?
{: #pricing-plan-satellite-non-deleted}

Recall from [Monitoring the lifecycle of encryption keys](/docs/key-protect?topic=key-protect-key-states) that keys can be either in the active, deactivated, suspended, or destroyed state. The act of deleting a key moves it to the destroyed state.

In the context of your key quota, you are charged for every key that is a state other than the destroyed state.