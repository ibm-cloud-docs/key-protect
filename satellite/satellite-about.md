---

copyright:
  years: 2023
lastupdated: "2023-04-13"

keywords: satellite, hsm, deploy

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

{{site.data.keyword.keymanagementservicefull}} on Satellite is a dedicated service that allows users to more fully control their own encryption keys by deploying {{site.data.keyword.keymanagementserviceshort}} into a [Satellite](/docs/satellite?topic=satellite-getting-started) location where users control their own infrastructure. {{site.data.keyword.keymanagementserviceshort}} on Satellite connects to a user owned and managed [hardware security module (HSM)](#x2009137){: term}, which gives users full control of the encryption keys and its operations within the HSM FIPS boundary.
{: shortdesc}

In this topic, references to "{{site.data.keyword.keymanagementserviceshort}}" refer to the {{site.data.keyword.keymanagementserviceshort}} on Satellite service.
{: note}

As protecting data everywhere becomes increasingly critical for organizations transforming into hybrid Cloud IT, {{site.data.keyword.cloud_notm}} is introducing the ability for {{site.data.keyword.keymanagementserviceshort}} to support data encryption on the new Satellite environments. {{site.data.keyword.keymanagementserviceshort}} on Satellite enables secure data services closer to where they are needed, with the additional security benefit of allowing customers to have separate and direct control of their "root of trust" with Bring Your Own Hardware Security Module (BYOHSM) capability.

To deploy {{site.data.keyword.keymanagementserviceshort}} on Satellite, you must be added to the allowlist. [Contact {{site.data.keyword.IBM_notm}}](https://www.ibm.com/contact/us/en/) to learn more.
{: important}

Because {{site.data.keyword.keymanagementserviceshort}} on Satellite features both a Satellite location with services (such as {{site.data.keyword.keymanagementserviceshort}}) managed by {{site.data.keyword.cloud_notm}} and infrastructure controlled by the user (including two fully user-controlled HSMs), it is designed for users who either desire or require closer control over their infrastructure (and keys) than is available with a fully managed service offering deployed on {{site.data.keyword.cloud_notm}}. This offering includes single tenant {{site.data.keyword.keymanagementserviceshort}} on Satellite that connects to two user-controlled HSMs, in contrast to the multi-tenant fully managed {{site.data.keyword.keymanagementserviceshort}} in {{site.data.keyword.cloud_notm}}.

This level of control is designed for users who want to promote internal best practices or satisfy relevant legal requirements. Users can use the same key management service on {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}, [on-prem](#x6969434){: term} {{site.data.keyword.keymanagementserviceshort}} on Satellite, and in future hybrid cloud environments, depending on the use case. This level of control is designed for businesses and industries where closer control over infrastructure satisfies business or regulatory needs (for example, a banking consortium subject to data residency rules).

For information about pricing, check out [Pricing for {site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-pricing-plan-satellite).
{: tip}

## Before you begin
{: #satellite-about-before-begin}

Before deploying {{site.data.keyword.keymanagementserviceshort}} in a Satellite location, you must first [deploy a Satellite location](/docs/satellite?topic=satellite-getting-started). Note that this location **must be [on-prem](#x6969434){: term}** in a user-owned [VM](##x2043253){: term}. It cannot be contained in {{site.data.keyword.cloud_notm}}.

After your Satellite location has been created, create an {{site.data.keyword.at_full_notm}} instance using the process described in [Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-before-begin-activity). You must also have deployed [a {{site.data.keyword.cloud_notm}} Databases (ICD) that uses PostgreSQL](/docs/cloud-databases?topic=cloud-databases-satellite-on-prem) service in your Satellite location, which is used by {{site.data.keyword.keymanagementserviceshort}} to manage your keys. After your ICD for PostgreSQL instance has deployed, you need to share [several values about your ICD instance](#satellite-about-before-begin-postgresql) with your service representative.
{: important}

The best practice is for {{site.data.keyword.keymanagementserviceshort}} to be deployed into an infrastructure containing at least three nodes with 8 vCPU and 32Gi of memory per node. These VM capacity numbers represent the aggregate capacity of all the micro-services that {{site.data.keyword.keymanagementserviceshort}} requires to run on your infrastructure. This aggregate capacity does not include what is needed for the two HSMs that you will manage. Additionally, [two HSMs will need to be deployed and accessible](#satellite-about-HSM). This infrastructure must be ready and accessible by Satellite before {{site.data.keyword.keymanagementserviceshort}} can be provisioned. This infrastructure continue to be the responsibility of the account owner, including the continuous management and maintenance of this infrastructure.

After you have created a Satellite location, an {{site.data.keyword.at_full_notm}} instance (in {{site.data.keyword.cloud_notm}}) and have installed PostgreSQL ICD in your Satellite location, and have deployed the rest of the prerequisite infrastructure (including your HSMs), you are ready to create {{site.data.keyword.keymanagementserviceshort}} in your Satellite location.

### Limitations and scope
{: #satellite-about-before-begin-limitations}

The first release of the {{site.data.keyword.keymanagementserviceshort}} on Satellite provides the basic functions to allow for secure handling of the encryption keys for the key services available in Satellite, such as {{site.data.keyword.cloud_notm}} Public (ROKS) and Cloud Object Storage (COS).

* At present, the only locations supported in this offering are in user-owned [on-prem](#x6969434){: term} Satellite locations associated with the {{site.data.keyword.cloud_notm}} `us-east` host Multi-Zone Region (MZR), though eventually it will be available in Satellite locations associated with all {{site.data.keyword.cloud_notm}} MZRs that Satellite supports. Note that each {{site.data.keyword.cloud_notm}} MZR can host multiple Satellite locations that are in close proximity to that MZR. While it is possible for a user to have their HSMs in another location as long as there is network connectivity (preferably on a private network) between the worker nodes hosting {{site.data.keyword.keymanagementserviceshort}} and your HSMs, the preferred method is a close physical proximity between HSMs and worker nodes.
* While the {{site.data.keyword.cloud_notm}} console is used to create the {{site.data.keyword.keymanagementserviceshort}} service on Satellite, the UI itself cannot currently be used to access the {{site.data.keyword.keymanagementserviceshort}} APIs that are used to create keys or perform other key actions (such as rotating keys, deleting keys, editing keys, and so on). Those key actions must be performed using the [{{site.data.keyword.keymanagementserviceshort}} CLI](/docs/key-protect?topic=key-protect-set-up-cli). Note that you must set the [`KP_PRIVATE_ADDR` environment variable](/docs/key-protect?topic=key-protect-private-endpoints) to the {{site.data.keyword.keymanagementserviceshort}} on Satellite endpoint for the CLI to work. You can also make direct calls to the [{{site.data.keyword.keymanagementserviceshort}} APIs](/apidocs/key-protect), also through the {{site.data.keyword.keymanagementserviceshort}} on Satellite endpoint. For more information about how to obtain the {{site.data.keyword.keymanagementserviceshort}} on Satellite endpoint from GhoST, check out [Obtaining the {{site.data.keyword.keymanagementserviceshort}} endpoint](#satellite-about-before-begin-endpoint). Note that keys you create using the CLI or API will not show up in the {{site.data.keyword.keymanagementserviceshort}} UI.
* All of the metrics that are typically available through {{site.data.keyword.cloud_notm}} Monitoring and are not available in the initial {{site.data.keyword.keymanagementserviceshort}} on Satellite release, but will be added in a future release. Because {{site.data.keyword.keymanagementserviceshort}} does not have the ability to receive logs from an HSM, the security monitoring of the HSMs are the responsibility of the owner of the HSM. To receive auditing logs, you must create an {{site.data.keyword.at_full_notm}} instance before provisioning {{site.data.keyword.keymanagementserviceshort}} on Satellite using the process described in [Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-before-begin-activity). Check out [Responsibilities](#satellite-about-before-begin-responsibilities) for more information about the responsibilities taken on by {{site.data.keyword.IBM_notm}} and those you must cover.
* Hyperwarp events are not available in Satellite. Typically, when a Hyperwarp event is not acknowledged, it is written as an event in {{site.data.keyword.at_full_notm}}. However, this is disabled in {{site.data.keyword.keymanagementserviceshort}} on Satellite because these Hyperwarp events are never sent in the first place.
* While you can [assign fine-grained access to a single key](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level), note that calling the [list keys API](/apidocs/key-protect#getkeys) will not return keys that you have been assigned individual access to (that only you can access, in other words). Calling this API will however return the keys in key rings you have access to (if you have access to all of the keys in an instance, you will see all keys). You can, however, see the keys that only you have access to by following the instructions in [Granting access to keys](/docs/key-protect?topic=key-protect-grant-access-keys) to view the key through IAM or by using the API to pass the specific key ID.

### Responsibilities
{: #satellite-about-before-begin-responsibilities}

The division of responsibilities between the end-user and {{site.data.keyword.IBM_notm}} for the {{site.data.keyword.keymanagementserviceshort}} on Satellite service are derived from the responsibilities inherent in Satellite itself, which can be found in [Your responsibilities](/docs/satellite?topic=satellite-responsibilities){: external}. Study this document carefully, as it outlines a number of important considerations and preparations a user must keep in mind when operating any service on Satellite.

In addition to understanding the responsibilities when using Satellite, there are a number of specific considerations to keep in mind when operating {{site.data.keyword.keymanagementserviceshort}} on Satellite.

If you're familiar with [Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities) (the fully managed {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}} service), you know that using {{site.data.keyword.keymanagementserviceshort}} on Satellite shifts several responsibilities from {{site.data.keyword.IBM_notm}} to you.

**User responsibilities**:

* Ensure your HSM is properly configured to work with {{site.data.keyword.keymanagementserviceshort}}. The details of the process to configure your HSM can only be obtained by contacting {{site.data.keyword.IBM_notm}} directly. Check out [Deploying or modifying an HSM to work with {{site.data.keyword.keymanagementserviceshort}}](#satellite-about-HSM) for more information.
* Allocate worker nodes and HSM as specified in [Before you begin](#satellite-about-before-begin).
* Ensure network connectivity between HSM and worker nodes has been established.
* Provision {{site.data.keyword.keymanagementserviceshort}} on Satellite using the instructions in [Deploying {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-ui-deploy).
* Give the {{site.data.keyword.keymanagementserviceshort}} service the following service-to-service authorizations in your Satellite location: [Satellite Cluster Creator](/docs/satellite?topic=satellite-iam#iam-resource-loc), [Satellite Link Source Access Controller](/docs/satellite?topic=satellite-iam#iam-resource-link) and [Satellite Link Administrator](/docs/satellite?topic=satellite-iam#iam-resource-link). Until these authorizations are given, it is not possible for a Satellite cluster to be created for {{site.data.keyword.keymanagementserviceshort}}. Once the authorization is set, {{site.data.keyword.keymanagementserviceshort}} is allowed to start the process of deploying {{site.data.keyword.keymanagementserviceshort}} on your worker nodes, a process that can take up to several hours. Once the deployment is completed, you will receive a completion status on your {{site.data.keyword.keymanagementserviceshort}} instance provision.
* When you update the operating System in your worker nodes, ensure that it does not disrupt the service. Check out [Change management](/docs/satellite?topic=satellite-infrastructure-service#satis-change-management) for more information.
* Updating [worker nodes](/docs/satellite?topic=satellite-hosts#host-update-workers) in your Satellite location.

**{{site.data.keyword.keymanagementserviceshort}} on Satellite responsibilities**:

* Running {{site.data.keyword.keymanagementserviceshort}} maintenance and updates in your Satellite location.
* Ensuring the security of key operations and key-related processes.
* Sending audit logs through [{{site.data.keyword.at_full_notm}}](#satellite-about-before-begin-activity).
* Support for issues related to the proper working of {{site.data.keyword.keymanagementserviceshort}}.

It is recommended to coordinate with the {{site.data.keyword.keymanagementserviceshort}} support team when maintaining your HSM, rotating keys, or performing other kinds of infrastructure maintenance.
{: tip}

### Using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.keymanagementserviceshort}}
{: #satellite-about-before-begin-activity}

All activity in {{site.data.keyword.keymanagementserviceshort}} is audited using {{site.data.keyword.at_full_notm}}. To get auditing logs through {{site.data.keyword.at_full_notm}}, you must first create an {{site.data.keyword.at_full_notm}} instance in {{site.data.keyword.cloud_notm}} and then pass an ingestion key during [the deployment of {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-satellite-ui-deploy). This ingestion key allows {{site.data.keyword.keymanagementserviceshort}} logs to be sent to your {{site.data.keyword.at_full_notm}} instance, where you can view them. For more information on {{site.data.keyword.at_full_notm}} events, check out [{{site.data.keyword.keymanagementserviceshort}} events](/docs/key-protect?topic=key-protect-at-events).

For more information on creating an ingestion key, check out the [{{site.data.keyword.IBM_notm}} Logging API reference doc](/apidocs/log-analysis#ingest){: external}.

In {{site.data.keyword.keymanagementserviceshort}} on Satellite, the `target.ResourceGroupID` value is always blank and the `target.name` is only visible if the resource targeted is a single {{site.data.keyword.keymanagementserviceshort}} key. To find the relevant events for your instance when either `target.ResourceGroupID` or `target.name` is returned blank, search using the cloud resource name (CRN) of your {{site.data.keyword.keymanagementserviceshort}} on Satellite instance. For more information about how to determine your CRN using the UI, CLI, or API, check out [Retrieving your instance ID and cloud resource name (CRN)](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
{: tip}

### Deploying {{site.data.keyword.cloud_notm}} Databases on Satellite
{: #satellite-about-before-begin-postgresql}

After you have [provisioned a PostgreSQL ICD instance into your account](/docs/cloud-databases?topic=cloud-databases-satellite-on-prem), you must extract information about your instance and share it with your service representative.

First, create a service key credential for the instance by issuing:

`ibmcloud resource service-key-create $service_key_name --instance-id $db_instance_guid`

Then issue the following query:

`ibmcloud resource service-key --output json $service_key_name`

This will output a lot of information. You will need six specific values:

1. db name (e.g., `ibmclouddb`), which can be found at: `credentials.connection.postgres.database`
2. db connection hostname, which can be found at: `credentials.connection.postgres.hosts.hostname`
3. db connection port number, which can be found at: `credentials.connection.postgres.hosts.port`
4. db authentication username, which can be found at: `credentials.connection.postgres.authentication.username`
5. db authentication password, which can be found at: `credentials.connection.postgres.authentication.password`
6. db connection certificate (in base64 encoded format), which can be found at: `credentials.connection.postgres.certificate.certificate_base64`

### Obtaining the {{site.data.keyword.keymanagementserviceshort}} endpoint
{: #satellite-about-before-begin-endpoint}

To use the CLI or the APIs to perform key actions against {{site.data.keyword.keymanagementserviceshort}} on Satellite, you must set the [`KP_PRIVATE_ADDR` environment variable](/docs/key-protect?topic=key-protect-private-endpoints) to the {{site.data.keyword.keymanagementserviceshort}} on Satellite endpoint and target the endpoint for your service. This endpoint is created dynamically during the creation process.

To get the endpoint of your service:

1. [Log in to the {{site.data.keyword.cloud_notm}} CLI](/login/){: external}.

2. Before you can set your Satellite location, you must retrieve it, which you can do by issuing: `ibmcloud sat endpoint get --location {Your_Satellite_Location} --endpoint KP-SATLINK-ENDPOINT --output json | jq -r .server_host`

3. You can then set your Satellite location by issuing: `export KP_PRIVATE_ADDR=<Value output from step 2>"`

### High availability and fault tolerance
{: #satellite-about-before-begin-ha}

Because your Satellite location is an extension of the host {{site.data.keyword.cloud_notm}} MZR, it maintains periodic communication to the host MZR using [{{site.data.keyword.cloud_notm}} Satellite Link connectivity](/docs/satellite?topic=satellite-link-location-cloud). To avoid {{site.data.keyword.keymanagementserviceshort}} availability impacts due to possible communication disruptions, it is important to follow high availability best practices.

{{site.data.keyword.keymanagementserviceshort}} caches authorization requests for up to 30 minutes to maintain availability to existing active users. As a result, if the link goes down for less than 30 minutes, existing active users will not be affected, since their authorizations can be verified from the cache. However, new policy changes (for example, to revoke access from a user) made during the down time will not be effective and {{site.data.keyword.keymanagementserviceshort}} will continue to allow access based on the previous policy. Similarly, users added during the down time remain unknown to {{site.data.keyword.keymanagementserviceshort}} and will be denied access until communication is restored.

Auditing logs cannot be sent to your [{{site.data.keyword.at_full_notm}} instance](#satellite-about-before-begin-activity) during the down time. However, these logs are stored and when the link has been restored will resume at the interruption point
{: important}

If the disruption between the Satellite location and {{site.data.keyword.cloud_notm}} lasts longer than 30 minutes, the authorization cache will expire and {{site.data.keyword.keymanagementserviceshort}} on Satellite will be unavailable. Similarly, if the storage limit for auditing logs is reached, additional logs can no longer be stored. At this point, only the logs that have already been stored are sent when service is resumed.

## Deploying or modifying an HSM to work with {{site.data.keyword.keymanagementserviceshort}}
{: #satellite-about-HSM}

As part of the infrastructure responsibility to support a Satellite location, you must deploy your own HSMs, configure them, and connect {{site.data.keyword.keymanagementserviceshort}} to them.

You can either procure a new HSM or configure a new partition in an existing HSM as long as it meets the following criteria:

* To ensure high availability, you must deploy two separate HSMs. You can create one or more partitions in each one of the HSMs. However, {{site.data.keyword.keymanagementserviceshort}} requires one partition from each HSM.
* For this release, {{site.data.keyword.keymanagementserviceshort}} only supports [Thales HSM A7xx models](https://thalesdocs.com/gphsm/luna/7.4/docs/network/Content/PDF_Network/Installation%20Guide.pdf). For more information about the models, firmware, and software versions that are supported, check out [About the HSMs supported for use with {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-hsm-deploy#satellite-hsm-supported). You are responsible for the procurement and general setup of these HSMs using the [Thales documentation](https://thalesdocs.com/gphsm/luna/7/docs/network/Content/Home_Luna.htm) and support infrastructure. Issues with the HSMs, unrelated to specific {{site.data.keyword.keymanagementserviceshort}} errors or bugs, must be handled through the Thales support process.
* The HSM (or the relevant partition) must be configured to create keys that {{site.data.keyword.keymanagementserviceshort}} can use, and using the least privileged access rule. To ensure the right configuration is followed it is recommended to engage with the {{site.data.keyword.IBM_notm}} support representative.

For more information about how to deploy an HSM (or create a partition in an HSM) that can work with {{site.data.keyword.keymanagementserviceshort}}, check out [Deploying an HSM to use with {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-satellite-hsm-deploy).

## Deploying {{site.data.keyword.keymanagementserviceshort}} on Satellite
{: #satellite-about-UI}

After your prerequisite infrastructure and HSMs have been deployed successfully and you have ensured that network connectivity between HSMs and worker nodes is up and running, you're ready to deploy {{site.data.keyword.keymanagementserviceshort}} and connect it to your HSM. For more information, check out [Deploying {{site.data.keyword.keymanagementserviceshort}} Satellite](/docs/key-protect?topic=key-protect-satellite-ui-deploy). Note that to successfully deploy {{site.data.keyword.keymanagementserviceshort}}, you will need to collect and provide [several values](/docs/key-protect?topic=key-protect-satellite-ui-deploy#satellite-ui-deploy-before-begin) from your HSM configuration.

Your {{site.data.keyword.cloud_notm}} account and {{site.data.keyword.keymanagementserviceshort}} APIs can still be used with {{site.data.keyword.keymanagementserviceshort}} on Satellite.
{: note}
