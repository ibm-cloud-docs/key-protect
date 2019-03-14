---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect availability, Key Protect disaster recovery

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# High availability and disaster recovery
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} is a highly available, regional service with automatic features that help keep your applications secure and operational.
{: shortdesc}

Use this page to learn more about {{site.data.keyword.keymanagementserviceshort}}'s availability and disaster recovery strategies.

## Locations, tenancy, and availability
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional service. 

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one of the supported [{{site.data.keyword.cloud_notm}} regions](/docs/services/key-protect/regions.html), which represent the geographic area where your {{site.data.keyword.keymanagementserviceshort}} requests are handled and processed. Each {{site.data.keyword.cloud_notm}} region contains [multiple availability zones ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/) to meet local access, low latency, and security requirements for the region.

As you plan your encryption at rest strategy with {{site.data.keyword.cloud_notm}}, keep in mind that provisioning {{site.data.keyword.keymanagementserviceshort}} in a region that is nearest to you is more likely to result in faster, more reliable connections when you interact with the {{site.data.keyword.keymanagementserviceshort}} APIs. Choose a specific region if the users, apps, or services that depend on a {{site.data.keyword.keymanagementserviceshort}} resource are geographically concentrated. Remember that users and services who are far away from the region might experience higher latency. 

Your encryption keys are confined to the region that you created them in. {{site.data.keyword.keymanagementserviceshort}} does not copy or export encryption keys to other regions.
{: note}

## Disaster recovery
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} has regional disaster recovery in place with a Recovery Time Objective (RTO) of one hour. The service follows {{site.data.keyword.cloud_notm}} requirements for planning and recovering from disaster events. For more information, see [Disaster recovery](/docs/overview/zero_downtime.html#disaster-recovery).


