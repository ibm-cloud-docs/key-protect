---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-27"

keywords: Key Protect availability, Key Protect disaster recovery

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
{:term: .term}

# High availability and disaster recovery
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} is a highly available, regional service with automatic features that help keep your applications secure and operational.
{: shortdesc}

Use this page to learn more about {{site.data.keyword.keymanagementserviceshort}}'s availability and disaster recovery strategies.

## Locations, tenancy, and availability
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional service. 

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one of the supported [{{site.data.keyword.cloud_notm}} regions](/docs/services/key-protect?topic=key-protect-regions#regions), which represent the geographic area where your {{site.data.keyword.keymanagementserviceshort}} requests are handled and processed. Each {{site.data.keyword.cloud_notm}} region contains [multiple availability zones](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external} to meet local access, low latency, and security requirements for the region.

As you plan your encryption at rest strategy with {{site.data.keyword.cloud_notm}}, keep in mind that provisioning {{site.data.keyword.keymanagementserviceshort}} in a region that is nearest to you is more likely to result in faster, more reliable connections when you interact with the {{site.data.keyword.keymanagementserviceshort}} APIs. Choose a specific region if the users, apps, or services that depend on a {{site.data.keyword.keymanagementserviceshort}} resource are geographically concentrated. Remember that users and services who are far away from the region might experience higher latency. 

Your encryption keys are confined to the region that you create them in. {{site.data.keyword.keymanagementserviceshort}} does not copy or export encryption keys to other regions.
{: note}

## Disaster recovery
{: #dr}

{{site.data.keyword.keymanagementserviceshort}} follows {{site.data.keyword.cloud_notm}} requirements for [planning and recovering from disaster events](/docs/overview?topic=overview-zero-downtime#disaster-recovery). To find out more about the responsibilities that you and IBM share for disaster recovery, see [Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery.)