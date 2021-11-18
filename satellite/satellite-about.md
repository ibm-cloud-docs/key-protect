<staging-satellite>---

copyright:
  years: 2021
lastupdated: "2021-11-18"

keywords: satellite, console, deploy

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
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# About Key Protect on Satellite
{: #satellite-about}

{{site.data.keyword.keymanagementservicefull}} on Satellite is a dedicated service that allows users to more fully control their own encryption keys by deploying {{site.data.keyword.keymanagementserviceshort}} into a [Satellite](/docs/satellite?topic=satellite-getting-started) location controlled by the users. {{site.data.keyword.keymanagementserviceshort}} on Satellite connects to a user owned and managed [hardware security module (HSM)](#x2009137){: term}, which gives users full control of the encryption keys and its operations within the HSM FIPS boundary.
{: shortdesc}

Because {{site.data.keyword.keymanagementserviceshort}} on Satellite features both a Satellite location managed by {{site.data.keyword.cloud_notm}} and controlled by the user with an HSM fully controlled and owned by the user, it is designed for users who either desire or require closer control over their infrastructure than is available with an offering deployed on {{site.data.keyword.cloud_notm}}. This includes single tenant {{site.data.keyword.keymanagementserviceshort}} with a user controlled HSM, unlike multi-tenant {{site.data.keyword.keymanagementserviceshort}} in {{site.data.keyword.cloud_notm}}.

This level of control is designed for users who want to promote internal best practices or satisfy relevant legal requirements. Users can use the same key management service on {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}, [on-prem](#x6969434){: term} {{site.data.keyword.keymanagementserviceshort}} on Satellite, and in future hybrid cloud environments, depending on the use case.

At present, {{site.data.keyword.keymanagementserviceshort}} is offered as a free service, though in the future all users will be migrated to a paid service. Contact your {{site.data.keyword.cloud_notm}} sales representative to learn more.
{: important}

## Before you begin
{: #satellite-about-before-begin}

Before deploying {{site.data.keyword.keymanagementserviceshort}} in a Satellite location, make sure to create an {{site.data.keyword.at_full_notm}} instance beforehand using the process described in [Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-before-begin-activity). You must also have deployed the {{site.data.keyword.cloud_notm}} Databases (ICD) service in your Satellite location, which is used by {{site.data.keyword.keymanagementserviceshort}} to manage your keys. If you do not have an {{site.data.keyword.cloud_notm}} Databases instance in your Satellite location, you can contact IBM support and request to use an ICD instance in {{site.data.keyword.cloud_notm}}.
{: important}

After you have created both your {{site.data.keyword.at_full_notm}} instance and your ICD instance, you can [deploy a Satellite location](/docs/satellite?topic=satellite-getting-started). Note that this location **must be [on-prem](#x6969434){: term}** in a user-owned [VM](##x2043253){: term}. It cannot be contained in a public cloud.

The best practice is for {{site.data.keyword.keymanagementserviceshort}} to be deployed into an infrastructure containing three nodes with 16 vCPU and 64Gi of memory per node.

These capacity numbers of the VMs represent aggregate capacity of all the micro-services that {{site.data.keyword.keymanagementserviceshort}} requires to run on your infrastructure. This aggregate capacity does not include what is needed for the two HSMs that you will manage.
{: tip}

After you have created an {{site.data.keyword.at_full_notm}} instance and a Satellite location, and have ensured than an ICD instance exists in your Satellite location, you can create {{site.data.keyword.keymanagementserviceshort}} in your Satellite location.

### Limitations
{: #satellite-about-before-begin-limitations}

As a first release of the {{site.data.keyword.keymanagementserviceshort}} on Satellite service, the capabilities of {{site.data.keyword.keymanagementserviceshort}} on Satellite are more limited than they will be in future releases.

At present, the only locations supported in this offering are in user-owned [on-prem](#x6969434){: term} Satellite locations associated with the {{site.data.keyword.cloud_notm}} `us-east` host multi-zone region (MZR), though eventually it will be available in Satellite locations associated with all {{site.data.keyword.cloud_notm}} MZRs that Satellite supports. Note that each {{site.data.keyword.cloud_notm}} MZR can host multiple Satellite locations that are in close proximity to that MZR. While it is possible for a user to have their HSMs in another location as long as there is network connectivity (preferably on a private network) between the worker nodes hosting {{site.data.keyword.keymanagementserviceshort}} and your HSMs, the preferred method is a close physical proximity between HSMs and worker nodes.

While the {{site.data.keyword.IBM_notm}} Console is used to create the {{site.data.keyword.keymanagementserviceshort}} service on Satellite, the console itself cannot currently be used to access the {{site.data.keyword.keymanagementserviceshort}} APIs that are used to create keys or perform other key actions (such as rotating keys, deleting keys, editing keys, and so on). Those key actions must be performed through direct calls to the [{{site.data.keyword.keymanagementserviceshort}} APIs](/apidocs/key-protect).

Metrics that are typically available through {{site.data.keyword.cloud_notm}} Monitoring and are not available in the initial release, but will be added in a future release. Because {{site.data.keyword.keymanagementserviceshort}} does not have the ability to receive logs from an HSM, the security monitoring of an HSM is responsibility of the owner of the HSM. To receive auditing logs, you must create an {{site.data.keyword.at_full_notm}} instance before provisioning {{site.data.keyword.keymanagementserviceshort}} on Satellite using the process described in [Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-before-begin-activity). Check out [Responsibilities](#satellite-about-before-begin-responsibilities) for more information about the responsibilities taken on by {{site.data.keyword.IBM_notm}} and those you must cover.

When a user of {{site.data.keyword.keymanagementserviceshort}} on Satellite [views lists of keys](/docs/key-protect?topic=key-protect-view-keys) through the [{{site.data.keyword.IBM_notm}} Console](https://cloud.ibm.com/login){: external}, or programmatically via the [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}, keys [which the logged-in user has individual access to](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level) won't appear due to the manner in which the service aggregates the collection. While the user can still use the key resource, only by using the CLI or API and passing the specific key ID can a user access the metadata and other details of the key.

{{site.data.keyword.keymanagementserviceshort}} only supports Satellite locations in `us-east`. Eventually, all regions will be supported.

### Responsibilities
{: #satellite-about-before-begin-responsibilities}

The responsibilities which must be taken by the user and by {{site.data.keyword.IBM_notm}} for {{site.data.keyword.keymanagementserviceshort}} on Satellite are themselves a subset of the responsibilities inherent in Satellite itself, which can be found in [Your responsibilities](/docs/satellite?topic=satellite-responsibilities){: external}. Study this document carefully, as it outlines a number of important considerations and preparations a user must keep in mind when operating any service on Satellite.

In addition to understanding the responsibilities when using Satellite, there are a number of specific considerations to keep in mind when operating {{site.data.keyword.keymanagementserviceshort}} on Satellite.

If you're familiar with [Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities), you know that using {{site.data.keyword.keymanagementserviceshort}} on Satellite shifts several responsibilities held by {{site.data.keyword.IBM_notm}} to you.

User responsibilities:

* Ensure your HSM is properly configured to work with {{site.data.keyword.keymanagementserviceshort}}. The details of the process to configure your HSM can only be obtained by contacting {{site.data.keyword.IBM_notm}} directly. The specifics are not contained in this documentation. Check out [Deploying or modifying an HSM to work with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-HSM) for more information.
* Allocate worker nodes and HSM as specified in [Before you begin](#satellite-about-before-begin).
* Ensure network connectivity between HSM and worker nodes has been established.
* Provision {{site.data.keyword.keymanagementserviceshort}} on Satellite using the instructions in [Deploying the Key Protect console to Satellite](/docs/key-protect?topic=key-protect-satellite-ui-deploy).
* Give the {{site.data.keyword.keymanagementserviceshort}} service authorization in your Satellite location. Since you need the {{site.data.keyword.keymanagementserviceshort}} instance ID for setting this "service to service" authorization, {{site.data.keyword.keymanagementserviceshort}} will return the ID with a "in progress" state. Once the authorization is set, {{site.data.keyword.keymanagementserviceshort}} is allowed to start the process of deploying {{site.data.keyword.keymanagementserviceshort}} on your worker nodes, a process that can take up to several hours. Once the deployment is completed, you will receive a completion status on your {{site.data.keyword.keymanagementserviceshort}} instance provision.
* When you update the operating System in your worker nodes, ensure that it does not disrupt the service. Check out [Change management](/docs/satellite?topic=satellite-infrastructure-service#satis-change-management) for more information.

{{site.data.keyword.keymanagementserviceshort}} responsibilities:

* Running the {{site.data.keyword.keymanagementserviceshort}} through the maintenance and updates to {{site.data.keyword.keymanagementserviceshort}} in your Satellite location.
* Ensuring the security of key operations and key-related processes.
* Sending audit logs through [{{site.data.keyword.at_full_notm}}](#satellite-about-before-begin-activity).
* Support for issues related to the proper working of {{site.data.keyword.keymanagementserviceshort}}.

It is recommended to coordinate with {{site.data.keyword.keymanagementserviceshort}} when you undertaking maintenance of your HSM, rotating keys, or performing other kinds of maintenance.
{: tip}

### Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}
{: #satellite-about-before-begin-activity}

{{site.data.keyword.at_full_notm}}

To get auditing logs through {{site.data.keyword.at_full_notm}}, you must first create an {{site.data.keyword.at_full_notm}} instance pass an ingestion key during [the deployment of {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-satellite-ui-deploy). This ingestion key allows {{site.data.keyword.keymanagementserviceshort}} logs to be sent to {{site.data.keyword.at_full_notm}}, where you can view them. For more information on {{site.data.keyword.at_full_notm}} events, check out [{{site.data.keyword.keymanagementserviceshort}} events](/docs/key-protect?topic=key-protect-at-events).

For more information on creating an ingestion key, check out the [{{site.data.keyword.IBM_notm}} Logging API reference doc](/apidocs/log-analysis#ingest){: external}.

### High availability
{: #satellite-about-before-begin-ha}

Because your Satellite location is connected to {{site.data.keyword.cloud_notm}} using [{{site.data.keyword.cloud_notm}} Direct Link connectivity](/docs/satellite?topic=satellite-link-location-cloud), it is expected that network disruption might occasionally occur between Satellite and {{site.data.keyword.cloud_notm}}. These disruptions pose a risk to the availability of {{site.data.keyword.keymanagementserviceshort}} on Satellite, and as a result the best practice is for users to mitigate these risks to the extent possible and be aware of the consequences.

Because {{site.data.keyword.keymanagementserviceshort}} caches authorization requests for up to 30 minutes, if the link goes down for less than 30 minutes, existing active users will not be affected, since their authorizations can be verified from the cache. However, if a policy change (for example, to revoke access from a user) is made during the down time, {{site.data.keyword.keymanagementserviceshort}} will not be aware of the policy change and will continue to allow access based on the previous policy. Similarly, users added during the down time will remain unknown to {{site.data.keyword.keymanagementserviceshort}} and will be denied access.

Auditing logs cannot be sent to your [{{site.data.keyword.at_full_notm}} instance](#satellite-about-before-begin-activity) during the down time. However, these logs are stored and when the link has been restored will resume at the interruption point
{: important}

If the link between the Satellite location and {{site.data.keyword.cloud_notm}} lasts longer than 30 minutes, the authorization cache will expire and {{site.data.keyword.keymanagementserviceshort}} on Satellite will be unavailable. Similarly, if the storage limit for auditing logs is reached, additional logs can no longer be stored. At this point, only the logs that have already been stored will be sent when service is resumed.

## Deploying or modifying an HSM to work with {{site.data.keyword.keymanagementserviceshort}}
{: #satellite-about-HSM}

While a Satellite location can be deployed in {{site.data.keyword.cloud_notm}}, you must deploy your own HSMs, configure them, and connect {{site.data.keyword.keymanagementserviceshort}} to them.

You can either purchase a new HSM or configure a new partition in an existing HSM as long as it meets certain criteria:

* To ensure high availability, you must deploy two separate HSMs. You can create one or more partitions in each one of the HSMs. However {{site.data.keyword.keymanagementserviceshort}} would need one partition from each HSM. Spreading these HSMs into different locations will increase their high availability.
* For this release, {{site.data.keyword.keymanagementserviceshort}} only supports [Thales HSM A7xx models](https://thalesdocs.com/gphsm/luna/7.4/docs/network/Content/PDF_Network/Installation%20Guide.pdf). For more information about the models, firmware, and software versions that are supported, check out [About the HSMs supported for use with {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-hsm-deploy#satellite-hsm-supported). You are responsible for the purchase and general setup of this HSM using the [Thales documentation] and support infrastructure. If you experience issues with this HSM unrelated to specific {{site.data.keyword.keymanagementserviceshort}} errors or bugs, you must consult the Thales support infrastructure to resolve issues.
* The HSM (or the relevant partition) must be configured to create keys that {{site.data.keyword.keymanagementserviceshort}} can use. The exact method used to achieve this configuration can be acquired by contacting {{site.data.keyword.IBM_notm}} and engaging with a support representative.

For more information about how to deploy an HSM (or create a partition in an HSM) that can work with {{site.data.keyword.keymanagementserviceshort}}, check out [Deploying an HSM to use with {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-hsm-deploy).

## Deploying the {{site.data.keyword.keymanagementserviceshort}} console
{: #satellite-about-UI}

After your HSM has been deployed successfully and you have ensured that network connectivity between HSMs and worker nodes is up and running, you're ready to deploy {{site.data.keyword.keymanagementserviceshort}} and connect it to your HSM. For more information, check out [Deploying the {{site.data.keyword.keymanagementserviceshort}} console to Satellite](/docs/key-protect?topic=key-protect-satellite-ui-deploy). Note that to successfully deploy {{site.data.keyword.keymanagementserviceshort}}, you will need to provide [several values](/docs/key-protect?topic=key-protect-satellite-ui-deploy#satellite-ui-deploy-before-begin) from your HSM.

Your {{site.data.keyword.cloud_notm}} account and {{site.data.keyword.keymanagementserviceshort}} APIs can still be used with {{site.data.keyword.keymanagementserviceshort}} on Satellite.
{: note}</staging-satellite>

