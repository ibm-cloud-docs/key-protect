---

copyright:
  years: 2019, 2020
lastupdated: "2020-01-15"

keywords: shared responsibilities, disaster recovery, incident management 

subcollection: key-protect
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}
{: #shared-responsibilities}
<!-- The title of your H1 should be Understanding your responsibilities with using _service-name_, where _service-name_ is the non-trademarked short version conref. -->

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.keymanagementservicefull}}. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{:shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.keymanagementservicelong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

  
## Incident and operations management
{: #incident-and-ops}

You and IBM share responsibilities for the set up and maintenance of your {{site.data.keyword.keymanagementservicelong_notm}} service instance for your application workloads. You are responsible for incident and operations management of your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Availability | Provide high availability capabilities, such as IBM-owned infrastructure in multizone regions, to meet local access and low latency requirements for each supported region. | Use the list of [available regions](/docs/services/key-protect?topic=key-protect-regions) to plan for and create new instances of the service. |
| Monitoring | Provide integration with select third-party partnership technologies, such as {{site.data.keyword.at_full}}. | Use the provided tools to [review instance logs and activities](/docs/services/key-protect?topic=key-protect-at-events). |
| Incidents | Provide notifications for planned maintenance, security bulletins, or unplanned outages.  | Set preferences to [receive emails about platform notifications](/docs/overview?topic=overview-ui#email-prefsl), and monitor the [IBM Cloud status page](https://{DomainName}/status?selected=announcement) for general announcements.
{: caption="Table 1. Responsibilites for incident and operations" caption-side="top"}


## Change management
{: #change-management}

You and IBM share responsibilities for keeping {{site.data.keyword.keymanagementservicelong_notm}} service components at the latest version. You are responsible for change management of your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Applications| Provide major, minor, and patch version updates for Key Protect interfaces. | Use the API, CLI, or console tools to apply the provided updates, including version updates, new features, and security patches. |
{: caption="Table 2. Responsibilites for change management" caption-side="top"}


## Identity and access management
{: #iam-responsibilities}

You and IBM share responsibilities for controlling access to your {{site.data.keyword.keymanagementservicelong_notm}} instances and resources. You are responsible for identity and access management to your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Applications| Provide the ability to restrict access to resources.  | Depending on your needs, restrict access to resources and service functionality by using Cloud IAM access policies. For more information, see [Managing user access](/docs/services/key-protect?topic=key-protect-manage-access).
{: caption="Table 3. Responsibilites for identity and access management" caption-side="top"}

## Security and regulation compliance
{: #security-compliance}

IBM is responsible for the security and compliance of {{site.data.keyword.keymanagementservicelong_notm}}. You are responsible for the security and compliance of your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Applications| Maintain controls that are commensurate to [various industry compliance standards](/docs/services/key-protect?topic=key-protect-security-and-compliance#compliance-ready), such as SOC and ISO.  | Set up and maintain security and regulation compliance for your apps and data. For example, you can enable extra security settings to meet your compliance needs by choosing how and when to [import](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead), [wrap](/docs/services/key-protect?topic=key-protect-wrap-keys), [rotate](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead), [rewrap](/docs/services/key-protect?topic=key-protect-rewrap-keys), and [delete](/docs/services/key-protect?topic=key-protect-delete-keys) keys. |
{: caption="Table 4. Responsibilites for security and regulation compliance" caption-side="top"}

## Disaster recovery
{: #disaster-recovery}

IBM is responsible for the recovery of {{site.data.keyword.keymanagementservicelong_notm}} components in case of disaster. You are responsible for the recovery of the workloads that run Key Protect and your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Applications | Continuously back up keys in the region that the service operates in, and automatically recover and restart service components after any disaster event. | When available, copy keys into a backup instance of Key Protect on a periodic basis. |
{: caption="Table 5. Responsibilites for disaster recovery" caption-side="top"}
