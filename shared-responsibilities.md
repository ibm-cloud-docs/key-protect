---

copyright:
  years: 2019, 2022
lastupdated: "2022-05-25"

keywords: shared responsibilities, disaster recovery, incident management

subcollection: key-protect
---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}
{: #shared-responsibilities}

Learn about the management responsibilities and terms and conditions that you
have when you use {{site.data.keyword.keymanagementservicefull}}.

For a high-level view of the service types in {{site.data.keyword.cloud_notm}}
and the breakdown of responsibilities between the customer and
{{site.data.keyword.IBM_notm}} for each type, see
[Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities){: external}.
{: shortdesc}

Review the following sections for the specific responsibilities for you and for
{{site.data.keyword.IBM_notm}} when you use
{{site.data.keyword.keymanagementservicelong_notm}}. For the overall terms of
use, see
[{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms){: external}.

## Incident and operations management
{: #incident-and-ops}

You and {{site.data.keyword.IBM_notm}} share responsibilities for the set up and maintenance of your
{{site.data.keyword.keymanagementservicelong_notm}} instance for your
application workloads.

You are responsible for incident and operations management of your application
data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Availability | Provide high availability capabilities, such as {{site.data.keyword.IBM_notm}}-owned infrastructure in multizone regions, to meet local access and low latency requirements for each supported region. | Use the list of [available regions](/docs/key-protect?topic=key-protect-regions) to plan for and create new instances of the service. |
| Monitoring | Provide integration with select third-party partnership technologies, such as {{site.data.keyword.at_full}}. | Use the provided tools to [review instance logs and activities](/docs/key-protect?topic=key-protect-at-events). |
| Incidents | Provide notifications for planned maintenance, security bulletins, or unplanned outages. | Set preferences to [receive emails about platform notifications](/docs/overview?topic=overview-ui#email-prefsl){: external}, and monitor the [{{site.data.keyword.IBM_notm}} Cloud status page](/status?selected=announcement){: external} for general announcements. |
{: caption="Table 1. Responsibilities for incident and operations" caption-side="top"}

## Change management
{: #change-management}

You and {{site.data.keyword.IBM_notm}} share responsibilities for keeping
{{site.data.keyword.keymanagementservicelong_notm}} service components at the
latest version.

You are responsible for change management of your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications| Provide major, minor, and patch version updates for {{site.data.keyword.keymanagementserviceshort}} interfaces. | Use the API, CLI, or console tools to apply the provided updates, including version updates, new features, and security patches. |
{: caption="Table 2. Responsibilities for change management" caption-side="top"}

## Identity and access management
{: #iam-responsibilities}

You and {{site.data.keyword.IBM_notm}} share responsibilities for controlling access to your
{{site.data.keyword.keymanagementservicelong_notm}} instances and resources.

You are responsible for identity and access management to your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications| Provide the ability to restrict access to resources. | Depending on your needs, restrict access to resources and service functionality by using Cloud IAM access policies. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access). |
{: caption="Table 3. Responsibilities for identity and access management" caption-side="top"}

## Security and regulation compliance
{: #security-compliance}

{{site.data.keyword.IBM_notm}} is responsible for the security and compliance of
{{site.data.keyword.keymanagementservicelong_notm}}.

You are responsible for the security and compliance of your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications| Maintain controls that are commensurate to [various industry compliance standards](/docs/key-protect?topic=key-protect-security-and-compliance#compliance-ready), such as SOC and ISO. | Set up and maintain security and regulation compliance for your apps and data. For example, you can enable extra security settings to meet your compliance needs by choosing how and when to [import](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead), [wrap](/docs/key-protect?topic=key-protect-wrap-keys), [rotate](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead), [rewrap](/docs/key-protect?topic=key-protect-rewrap-keys), and [delete](/docs/key-protect?topic=key-protect-delete-keys) keys. |
{: caption="Table 4. Responsibilities for security and regulation compliance" caption-side="top"}

## Disaster recovery
{: #disaster-recovery}

{{site.data.keyword.IBM_notm}} is responsible for the recovery of
{{site.data.keyword.keymanagementservicelong_notm}} components in case of
disaster.

You are responsible for the recovery of the workloads that run
{{site.data.keyword.keymanagementserviceshort}} and your application data.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Continuously back up keys in the region that the service operates in, and automatically recover and restart service components after any disaster event. | None. {{site.data.keyword.IBM_notm}} and {{site.data.keyword.keymanagementserviceshort}} are fully responsible for managing disaster recovery. |
| Virtual Private Endpoints (VPE) | VPE does not support automatically switching to backup during failover at this time. | VPE settings, specifically the Internet Protocol (IP) address, need to be manually updated during disaster recovery procedures. |
| Private Endpoint (PE) | PE will not support allowed IP settings during disaster recovery at this time, but an announcement on this topic will be made soon. | PE settings, specifically the Internet Protocol (IP) address, need to be manually updated during disaster recovery procedures. |
{: caption="Table 5. Responsibilities for disaster recovery" caption-side="top"}


