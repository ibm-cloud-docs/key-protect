---

copyright:
  years: 2019, 2026
lastupdated: "2026-03-19"

keywords: shared responsibilities, disaster recovery, incident management

subcollection: key-protect
---

{{site.data.keyword.attribute-definition-list}}

# Your responsibilities when using {{site.data.keyword.keymanagementserviceshort}}
{: #shared-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).



Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.keymanagementservicelong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).


This document also includes comparative information about {{site.data.keyword.keymanagementserviceshort}} Standard and {{site.data.keyword.keymanagementserviceshort}} Dedicated to help you understand the differences in shared responsibilities between the two service tiers.
{: tip}


## Incident and operations management
{: #incident-and-ops}

You and {{site.data.keyword.IBM_notm}} share responsibilities for the set up and maintenance of your {{site.data.keyword.keymanagementservicelong_notm}} instance for your application workloads.

You are responsible for incident and operations management of your application data.

### {{site.data.keyword.keymanagementserviceshort}} Standard responsibilities
{: #incident-and-ops-kp}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Availability | Provide high availability capabilities, such as {{site.data.keyword.IBM_notm}}-owned infrastructure in multizone regions, to meet local access and low latency requirements for each supported region. | Use the list of [available regions](/docs/key-protect?topic=key-protect-regions) to plan for and create new instances of the service. |
| Monitoring | Provide integration with select third-party partnership technologies, such as {{site.data.keyword.logs_full_notm}}. | Use the provided tools to [review instance logs and activities](/docs/cloud-logs). |
| Incidents | Provide notifications for planned maintenance, security bulletins, or unplanned outages. | Set preferences to [receive emails about platform notifications](/docs/account?topic=account-email-prefs), and monitor the [{{site.data.keyword.IBM_notm}} Cloud status page](/status?selected=announcement) for general announcements. |
{: caption="Table 1. Responsibilities for incident and operations - Key Protect Standard" caption-side="bottom"}


### {{site.data.keyword.keymanagementserviceshort}} Dedicated responsibilities
{: #incident-and-ops-hpcs}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Monitoring | Provide integration with select third-party partnership technologies, such as {{site.data.keyword.cloud_notm}} Activity Tracker. | Use the provided tools to [review instance logs and activities](/docs/key-protect?topic=key-protect-at-events). |
| Incidents | Provide notifications for planned maintenance, security bulletins, or unplanned outages. | Set preferences to [receive emails about platform notifications](/docs/account?topic=account-email-prefs), and monitor the [IBM Cloud status page](https://cloud.ibm.com/status?selected=announcement) for general announcements. |
| Firmware updates | Provide firmware updates for multiple crypto units in a sequential manner with no impact to running workloads. | Perform tasks as usual. |
| Connecting to third-party cloud | Provide error messages when access to the third-party keystores does not work. | Contact affected third-party cloud service provider to resolve access or connection issues. |
{: caption="Table 2. Responsibilities for incident and operations - Key Protect Dedicated" caption-side="bottom"}



## Change management
{: #change-management}

You and {{site.data.keyword.IBM_notm}} share responsibilities for keeping {{site.data.keyword.keymanagementservicelong_notm}} service components at the latest version.

You are responsible for change management of your application data.

### {{site.data.keyword.keymanagementserviceshort}} Standard responsibilities
{: #change-management-kp}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Provide major, minor, and patch version updates for {{site.data.keyword.keymanagementserviceshort}} interfaces. | Use the API, CLI, or console tools to apply the provided updates, including version updates, new features, and security patches. |
{: caption="Table 3. Responsibilities for change management - Key Protect Standard" caption-side="bottom"}


### {{site.data.keyword.keymanagementserviceshort}} Dedicated responsibilities
{: #change-management-hpcs}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Provide major, minor, and patch version updates for {{site.data.keyword.keymanagementserviceshort}} Dedicated interfaces. | Use the API, CLI, or console tools to perform updated functions. |
{: caption="Table 4. Responsibilities for change management - Key Protect Dedicated" caption-side="bottom"}


## Identity and access management
{: #iam-responsibilities}

You and {{site.data.keyword.IBM_notm}} share responsibilities for controlling access to your {{site.data.keyword.keymanagementservicelong_notm}} instances and resources.

You are responsible for identity and access management to your application data.

### {{site.data.keyword.keymanagementserviceshort}} Standard responsibilities
{: #iam-responsibilities-kp}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Provide the ability to restrict access to resources. | Depending on your needs, restrict access to resources and service functionality by using Cloud IAM access policies. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access). |
{: caption="Table 5. Responsibilities for identity and access management - Key Protect Standard" caption-side="bottom"}


### {{site.data.keyword.keymanagementserviceshort}} Dedicated responsibilities
{: #iam-responsibilities-hpcs}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| User management | IBM does not have access to your resources but provides you with the ability to restrict user access to resources. | Depending on your needs, restrict access to resources and service functionality by using Cloud IAM access policies. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access). |
| Service and key policies | IBM does not have access to your resources but provides you with the ability to set service and key policies. | Depending on your needs, set service and key policies to control access to your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance and keys. For more information, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access). |
| Connecting to third-party cloud | Provide error messages when access to the third-party keystores does not work. | Make sure the connection information and access credentials for the third-party cloud service provider are always up to date in your {{site.data.keyword.keymanagementserviceshort}} Dedicated instance. |
{: caption="Table 6. Responsibilities for identity and access management - Key Protect Dedicated" caption-side="bottom"}



## Security and regulation compliance
{: #security-compliance}

{{site.data.keyword.IBM_notm}} is responsible for the security and compliance of {{site.data.keyword.keymanagementservicelong_notm}}.

You are responsible for the security and compliance of your application data.

### {{site.data.keyword.keymanagementserviceshort}} Standard responsibilities
{: #security-compliance-kp}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Maintain controls that are commensurate to [various industry compliance standards](/docs/key-protect?topic=key-protect-security-and-compliance#compliance-ready), such as SOC and ISO. | Set up and maintain security and regulation compliance for your apps and data. For example, you can enable extra security settings to meet your compliance needs by choosing how and when to [import](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead), [wrap](/docs/key-protect?topic=key-protect-wrap-keys), [rotate](/docs/key-protect?topic=key-protect-importing-keys#plan-ahead), [rewrap](/docs/key-protect?topic=key-protect-rewrap-keys), and [delete](/docs/key-protect?topic=key-protect-delete-keys) keys. |
{: caption="Table 7. Responsibilities for security and regulation compliance - Key Protect Standard" caption-side="bottom"}


### {{site.data.keyword.keymanagementserviceshort}} Dedicated responsibilities
{: #security-compliance-hpcs}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Maintain controls that are commensurate to [various industry compliance standards](/docs/key-protect?topic=key-protect-security-and-compliance#compliance-ready), such as SOC and ISO. | Set up and maintain security and regulation compliance for your apps and data. For example, you can enable extra security settings to meet your compliance needs by choosing how and when to [import](/docs/key-protect?topic=key-protect-import-root-keys), [wrap](/docs/key-protect?topic=key-protect-wrap-keys), [rotate](/docs/key-protect?topic=key-protect-rotate-keys), [rewrap](/docs/key-protect?topic=key-protect-rewrap-keys), and [delete](/docs/key-protect?topic=key-protect-delete-keys) keys. |
| Cryptographic operations | IBM never touches your master key. | Keep your master key secure. Perform cryptographic operations by using your workstation or smart cards. |
| Master key backups | IBM never touches your master key. | Back up your master key in a regular basis to your smart card or workstation. |
| Smart cards and smart card readers | IBM never touches your smart cards and smart card readers. | Store smart cards, the associated PINs, and the smart card readers in secure vaults to prevent unauthorized access. |
| Key part files | IBM never touches your key part files. | Secure key part files in the directory associated with CLOUDTKEFILES in an approved file storage vault and securely store the respective passwords. |
{: caption="Table 8. Responsibilities for security and regulation compliance - Key Protect Dedicated" caption-side="bottom"}



## Disaster recovery
{: #disaster-recovery}

{{site.data.keyword.IBM_notm}} is responsible for the recovery of {{site.data.keyword.keymanagementservicelong_notm}} components in case of disaster.

You are responsible for the recovery of the workloads that run {{site.data.keyword.keymanagementserviceshort}} and your application data.

### {{site.data.keyword.keymanagementserviceshort}} Standard responsibilities
{: #disaster-recovery-kp}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Applications | Continuously back up keys in the region that the service operates in, and automatically recover and restart service components after any disaster event. | None. {{site.data.keyword.IBM_notm}} and {{site.data.keyword.keymanagementserviceshort}} are fully responsible for managing disaster recovery. |
| Virtual Private Endpoints (VPE) | VPE does not support automatically switching to backup during failover at this time. | VPE settings, specifically the Internet Protocol (IP) address, need to be manually updated during disaster recovery procedures. |
| Private Endpoint (PE) | PE will not support allowed IP settings during disaster recovery at this time, but an announcement on this topic will be made soon. | PE settings, specifically the Internet Protocol (IP) address, need to be manually updated during disaster recovery procedures. |
{: caption="Table 9. Responsibilities for disaster recovery - Key Protect Standard" caption-side="bottom"}


### {{site.data.keyword.keymanagementserviceshort}} Dedicated responsibilities
{: #disaster-recovery-hpcs}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
| ---- | ----------------------------------------------- | --------------------- |
| Instance backups | Continuously perform in-region and cross-region backups of key resources and perform continuous testing of backups. | Back up your master key; validate the backups and restore data when needed. |
| Disaster recovery | When an in-region disaster occurs, automatically recover and restart service components. When a regional disaster that affects all available zones occurs, ensure that all data except the master key is replicated to another region. IBM will also make additional crypto units available in a different region and manage routing requests to the new crypto units. | When a regional disaster that affects all available zones occurs, load your master key to the new crypto units that IBM provisions in a different region for restoring data. You can also enable and initialize failover crypto units before a disaster occurs, which reduces the downtime. |
| Availability | Provide high availability capabilities, such as IBM-owned infrastructure in multizone regions, to meet local access and low latency requirements for each supported region. | Use the list of [available regions](/docs/key-protect?topic=key-protect-regions) to plan for and create new instances of the service. |
{: caption="Table 10. Responsibilities for disaster recovery - Key Protect Dedicated" caption-side="bottom"}
