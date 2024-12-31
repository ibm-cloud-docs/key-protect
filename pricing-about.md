---

copyright:
  years: 2024
lastupdated: "2024-12-17"

keywords: pricing plan, billing, cost

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

# About pricing
{: #pricing-plan-about}
{: ui}
{: api}
{: cli}

The pricing model for {{site.data.keyword.keymanagementserviceshort}} depends on either where the service is deployed or the disaster recovery capabilities it has, reflecting the differing use cases and infrastructure requirements of your deployments.
{: shortdesc}

If your instance is deployed in an {{site.data.keyword.cloud_notm}} location, you are charged per account based on how many key versions you have in all of your {{site.data.keyword.keymanagementserviceshort}} instances. If you have selected a plan that has enhanced cross-regional disaster recovery, you also are charged a flat monthly rate for each region where you have instances in addition to being charged double the key version rate. If you select a plan that does not have enhanced cross-regional support, you are not charged for instances. 

If the service is deployed into a user-managed Satellite location, you are charged a flat rate for the service (regardless of the number of instances) and an additional rate for the total allotment of keys (also known as your “quota”) you have selected for that location.

For more information about pricing for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}, check out [Pricing for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}](/docs/key-protect?topic=key-protect-pricing-plan).

For more information about pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite, check out [Pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-pricing-plan-satellite).
