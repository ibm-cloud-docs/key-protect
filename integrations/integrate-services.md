---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-20"

keywords: Key Protect integration, integrate service with Key Protect

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

# Integrating services
{: #integrate-services}

{{site.data.keyword.keymanagementservicefull}} integrates with a number of
{{site.data.keyword.cloud_notm}} services to enable encryption with
customer-managed keys for those services. Encryption with customer-managed
encryption keys is sometimes called Bring Your Own Key (BYOK).
{: shortdesc}

## Database service integrations
{: #database-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the
following **database** services.

| Service | Description | Links |
| ------- | ----------- | ----- |
| [{{site.data.keyword.cloudant_short_notm}} for {{site.data.keyword.cloud_notm}} ({{site.data.keyword.cloud_notm}} Dedicated)](/docs/Cloudant?topic=Cloudant-ibm-cloud-dedicated){: external} | {{site.data.keyword.cloudant_short_notm}} is a document-oriented database as a service (DBaaS). It stores data as documents in JSON format. | [View docs](/docs/Cloudant?topic=Cloudant-securing-your-data-in-cloudant){: external} |
| [{{site.data.keyword.databases-for-cassandra_full_notm}}](/docs/databases-for-cassandra?topic=databases-for-cassandra-getting-started){: external} | {{site.data.keyword.databases-for-cassandra_full_notm}}, an enterprise-grade Apache Cassandra cloud solution with high performance, workload flexibility, and proven reliability. | [View docs](/docs/databases-for-cassandra?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-elasticsearch_full_notm}}](/docs/databases-for-elasticsearch){: external} | {{site.data.keyword.databases-for-elasticsearch_full_notm}} is a managed Elasticsearch service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-elasticsearch?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-enterprisedb_full_notm}}](/docs/databases-for-enterprisedb?topic=databases-for-enterprisedb-getting-started){: external} | {{site.data.keyword.databases-for-enterprisedb_full_notm}} offers a single source for purchase, deployment and support of EDB Postgres Enterprise and Standard editions based on PostgreSQL. | [View docs](/docs/databases-for-enterprisedb?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-etcd_full_notm}}](/docs/databases-for-etcd){: external} | {{site.data.keyword.databases-for-etcd_full_notm}} is a managed etcd service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-etcd?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-mongodb_full_notm}}](/docs/databases-for-mongodb){: external} | {{site.data.keyword.databases-for-mongodb_full_notm}} is a managed MongoDB service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-mongodb?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-mysql_fullnotm}}](/docs/databases-for-mysql){: external} | {{site.data.keyword.databases-for-mongodb_full_notm}} is a managed MySQL service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-mysql?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-postgresql_full_notm}}](/docs/databases-for-postgresql){: external} | {{site.data.keyword.databases-for-postgresql_full_notm}} is a managed PostgreSQL service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-postgresql?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.databases-for-redis_full_notm}}](/docs/databases-for-redis){: external} | {{site.data.keyword.databases-for-redis_full_notm}} is a managed service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/databases-for-redis?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}](/docs/hyper-protect-dbaas-for-mongodb){: external} | {{site.data.keyword.ihsdbaas_mongodb_full}} offers fully managed and highly secure {{site.data.keyword.mongodb}} databases with a high level of data confidentiality for your sensitive data. | [View docs](/docs/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-key-protect-byok){: external} |
| [{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_postgresql_full}}](/docs/hyper-protect-dbaas-for-postgresql){: external} | {{site.data.keyword.ihsdbaas_postgresql_full}} offers fully managed and highly secure {{site.data.keyword.postgresql}} databases with a high level of data confidentiality for your sensitive data. | [View docs](/docs/hyper-protect-dbaas-for-postgresql?topic=hyper-protect-dbaas-for-postgresql-key-protect-byok){: external} |
| [{{site.data.keyword.messages-for-rabbitmq_full_notm}}](/docs/messages-for-rabbitmq){: external} | {{site.data.keyword.messages-for-rabbitmq_full_notm}} is a managed RabbitMQ service that is hosted in the {{site.data.keyword.cloud_notm}} and integrated with other {{site.data.keyword.cloud_notm}} services. | [View docs](/docs/messages-for-rabbitmq?topic=cloud-databases-key-protect){: external} |
| [{{site.data.keyword.Db2_on_Cloud_long_notm}}](/docs/Db2onCloud){: external} | {{site.data.keyword.Db2_on_Cloud_long_notm}} is an SQL database that is provisioned for you in the cloud. You can use {{site.data.keyword.Db2_on_Cloud_short}} just as you would use any database software, but without the time and expense of hardware setup or software installation and maintenance. | [View docs](/docs/Db2onCloud?topic=Db2onCloud-key-protect-v2){: external}  |
| [{{site.data.keyword.sqlquery_short}}](/docs/sql-query){: external}  | You can use the {{site.data.keyword.sqlquery_short}} service to run SQL queries (that is, SELECT statements) to analyze, transform, or clean up rectangular data. | [View docs](/docs/sql-query?topic=sql-query-keyprotect){: external}  |
{: caption="Table 1. Supported database services." caption-side="bottom"}

## Storage service integrations
{: #storage-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the
following **storage** services.

| Service | Description | Integration docs |
| ------- | ----------- | ---------------- |
| [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-getting-started){: external} | You can use {{site.data.keyword.block_storage_is_short}} to provide hypervisor-mounted, high-performance data storage for virtual server instances in your VPC. | [View docs](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption){: external} |
| [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage){: external} | You can use {{site.data.keyword.cos_full_notm}} to store unstructured data in the {{site.data.keyword.cloud_notm}}. | [View docs](/docs/cloud-object-storage?topic=cloud-object-storage-kp){: external} |
{: caption="Table 2. Supported storage services." caption-side="bottom"}

## Compute service integrations
{: #compute-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the following **compute** services.

| Service | Description | Integration docs |
| ------- | ----------- | ---------------- |
| [{{site.data.keyword.cloud_notm}} image templates](/docs/image-templates?topic=image-templates-getting-started-with-image-templates#getting-started-with-image-templates){: external} | You can use {{site.data.keyword.cloud_notm}} image templates to capture an image of a virtual server to quickly replicate its configuration with minimal changes in the order process. With the End to End (E2E) Encryption feature, you can bring your own encrypted, cloud-init enabled operating system image. | [View docs](/docs/image-templates?topic=image-templates-using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance#using-end-to-end-e2e-encryption-to-provision-an-encrypted-instance){: external} |
| [KMIP for VMware](/docs/vmwaresolutions?topic=vmwaresolutions-kmip_standalone_considerations){: external} | KMIP for VMware works together with VMware native vSphere encryption and vSAN encryption to provide simplified storage encryption management together with the security and flexibility of {{site.data.keyword.keymanagementserviceshort}} or Hyper Protect Crypto Services customer-managed keys. | [View docs](/docs/vmwaresolutions?topic=vmwaresolutions-kmip_standalone_considerations){: external} |
| [{{site.data.keyword.vsi_is_short}}](/docs/vpc?topic=vpc-getting-started){: external} | You can use {{site.data.keyword.vsi_is_short}} to create an instance that consists of your virtual compute resources and resulting capacity within an {{site.data.keyword.vpc_short}}. | [View docs](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console){: external}
| [{{site.data.keyword.powerSys_notm}}](/docs/power-iaas?topic=power-iaas-getting-started){: external} | You can protect Linux Unified Key Setup (LUKS) encryption keys from being compromised by using {{site.data.keyword.keymanagementserviceshort}}. |[View docs](https://developer.ibm.com/tutorials/protect-luks-encryption-keys-with-ibm-cloud-hyper-protect-crypto-services/){: external} |
| [{{site.data.keyword.discoveryshort}}](/docs/discovery?topic=discovery-getting-started){: external} | You can use {{site.data.keyword.discoveryshort}} to build cognitive, cloud-based exploration applications that analyze and provide new insights within your data. | [View docs](/docs/discovery?topic=watson-keyservice){: external} |
| [{{site.data.keyword.languagetranslationshort}}](/docs/language-translator?topic=language-translator-gettingstarted){: external} | You can use {{site.data.keyword.languagetranslationshort}} to identify the language in text and convert it into other languages. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.speechtotextshort}}](/docs/speech-to-text?topic=speech-to-text-gettingStarted){: external} | You can use {{site.data.keyword.speechtotextshort}} to create customizable speech recognition for optimal text transcription in your application. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.texttospeechshort}}](/docs/text-to-speech?topic=text-to-speech-gettingStarted){: external} | You can use {{site.data.keyword.texttospeechshort}}'s speech-synthesis capabilities to convert written text into natural-sounding speech. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.toneanalyzershort}}](/docs/tone-analyzer?topic=tone-analyzer-gettingStarted){: external} | You can use {{site.data.keyword.toneanalyzershort}} to detect emotional and language tones in your written texts. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.knowledgestudioshort}}](/docs/tone-analyzer?topic=tone-analyzer-gettingStarted){: external} | You can use {{site.data.keyword.knowledgestudioshort}} to understand the linguistic nuances, meaning, and relationships specific to your industry. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.aios_short}}](https://dataplatform.cloud.ibm.com/docs/content/wsj/model/getting-started.html){: external} | You can use {{site.data.keyword.aios_short}} to automate and maintain the AI lifecycle in your business applications. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.conversationfull}}](/docs/assistant?topic=assistant-getting-started#getting-started){: external} | You can use {{site.data.keyword.conversationfull}} to to build your own branded live chatbot into any device, application, or channel. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.nlufull}}](/docs/natural-language-understanding?topic=natural-language-understanding-getting-started#getting-started){: external} | You can use {{site.data.keyword.nlufull}} to analyze semantic features of text input, including categories, concepts, emotion, entities, keywords, metadata, relations, semantic roles, and sentiment. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
| [{{site.data.keyword.personalityinsightsfull}}](/docs/natural-language-understanding?topic=natural-language-understanding-about){: external} | You can use {{site.data.keyword.personalityinsightsfull}} to analyze semantic features of text input, including categories, concepts, emotion, entities, keywords, metadata, relations, semantic roles, and sentiment. | [View docs](/docs/watson?topic=watson-keyservice){: external} |
{: caption="Table 3. Supported compute services." caption-side="bottom"}

## Container service integrations
{: #container-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the
following **container** services.

| Service | Description | Integration docs |
| ------- | ----------- | ---------------- |
| [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started){: external} | You can use the {{site.data.keyword.containerlong_notm}} service to deploy highly available apps in Docker containers that run in Kubernetes clusters. | [View docs](/docs/containers?topic=containers-encryption#keyprotect){: external} |
| [{{site.data.keyword.openshiftlong_notm}}](https://cloud.ibm.com/docs/openshift?topic=openshift-encryption){: external} | You can use the {{site.data.keyword.openshiftlong_notm}} service to deploy secure, highly available apps in OpenShift clusters. | [View docs](https://cloud.ibm.com/docs/openshift?topic=openshift-encryption){: external} |
{: caption="Table 4. Supported container services." caption-side="bottom"}

## Ingestion service integrations
{: #integration-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the
following **integration** services.

| Service | Description | Integration docs |
| ------- | ----------- | ---------------- |
| [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started){: external} | The {{site.data.keyword.mon_full_notm}} service is a container-intelligence management system. You can use it to gain operational visibility into the performance and health of your applications, services, and platforms. | [View docs](/docs/monitoring?topic=monitoring-mng-data){: external} |
| [{{site.data.keyword.bpfull_notm}}](/docs/schematics?topic=schematics-getting-started){: external} | The {{site.data.keyword.bpfull_notm}} service delivers Terraform-as-a-Service. You can use it to organize your {{site.data.keyword.cloud_notm}} resources across environments by using workspaces. | [View docs](/docs/schematics?topic=schematics-secure-data#pi-encrypt) |
| [{{site.data.keyword.messagehub_full}}](/docs/EventStreams?topic=EventStreams-getting_started){: external} | The {{site.data.keyword.messagehub}} service is a high-throughput message bus built with Apache Kafka. You can use it for event ingestion into {{site.data.keyword.cloud_notm}} and event stream distribution between your services and applications. | [View docs](/docs/EventStreams?topic=EventStreams-managing_encryption){: external} |
{: caption="Table 5. Supported integration services." caption-side="bottom"}

## Developer tools service integrations
{: #integration-devtools-integrations}

You can integrate {{site.data.keyword.keymanagementserviceshort}} with the
following **developer tools** services.

| Service | Description | Integration docs |
| ------- | ----------- | ---------------- |
| [{{site.data.keyword.cloud_notm}} {{site.data.keyword.contdelivery_short}}](/docs/ContinuousDelivery?topic=ContinuousDelivery-getting-started){: external} | The {{site.data.keyword.contdelivery_short}} service provides a suite of tools that support DevOps best practices. You can use the service to manage toolchains, operate delivery pipelines, gain insights into code quality and vulnerabilities, integrate third party tools, and more. | [Creating a Continuous Delivery service instance](/docs/ContinuousDelivery?topic=ContinuousDelivery-create_cd_service){: external} [Protecting your personal data when you use the Professional plan](/docs/ContinuousDelivery?topic=ContinuousDelivery-cd_data_security#cd_professional_plan){: external} |
{: caption="Table 6. Supported developer tools services." caption-side="bottom"}

## Understanding your integration
{: #understand-integration}

When you integrate a supported service with
{{site.data.keyword.keymanagementserviceshort}}, you enable
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)
for that service. This integration allows you to use a root key that you store
in {{site.data.keyword.keymanagementserviceshort}} to wrap the data encryption
keys that encrypt your data at rest.

For example, you can create a root key, manage the key in
{{site.data.keyword.keymanagementserviceshort}}, and use the root key to protect
the data that is stored across different cloud services.

![The diagram shows a contextual view of your {{site.data.keyword.keymanagementserviceshort}} integration.](../images/kp-integrations.svg){: caption="Figure 1. Contextual view of {{site.data.keyword.keymanagementserviceshort}} integration." caption-side="bottom"}

### {{site.data.keyword.keymanagementserviceshort}} API methods
{: #envelope-encryption-api-methods}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API
drives the envelope encryption process.

The following table lists the API methods that add or remove envelope encryption
on a resource.

| Method | Description |
| ------ | ----------- |
| `POST /keys/{root_key_ID}/actions/wrap` | [Wrap (encrypt) a data encryption key](/docs/key-protect?topic=key-protect-wrap-keys) |
| `POST /keys/{root_key_ID}/actions/unwrap` | [Unwrap (decrypt) a data encryption key](/docs/key-protect?topic=key-protect-unwrap-keys) |
{: caption="Table 6. Describes the {{site.data.keyword.keymanagementserviceshort}} API methods." caption-side="bottom"}

To find out more about programmatically managing your keys in
{{site.data.keyword.keymanagementserviceshort}}, check out the
[{{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
{: tip}

## Integrating a supported service
{: #grant-access}

To add an integration, create an authorization between services by using the
{{site.data.keyword.iamlong}} dashboard. Authorizations enable service to
service access policies, so you can associate a resource in your cloud data
service with a
[root key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types)
that you manage in {{site.data.keyword.keymanagementserviceshort}}.

Be sure to provision both services in the same region before you create an
authorization. To learn more about service authorizations, see
[Granting access between services](/docs/account?topic=account-serviceauth){: external}.
{: note}

When you're ready to integrate a service, use the following steps to create an
authorization:

1. From the menu bar, click **Manage** &gt; **Access (IAM)**, and select
    **Authorizations**.

2. Click **Create**.

3. Select a source and target service for the authorization.

    For **Source service**, select the cloud data service that you want to
    integrate with {{site.data.keyword.keymanagementserviceshort}}

    For **Target service**, select
    **{{site.data.keyword.keymanagementservicelong_notm}}**.

4. Enable the **Reader** role.

    With _Reader_ permissions, your source service can browse the root keys that
    are provisioned in the specified instance of
    {{site.data.keyword.keymanagementserviceshort}}.

5. Click **Authorize**.

## What's next
{: #integration-next-steps}

Add advanced encryption to your cloud resources by creating a root key in
{{site.data.keyword.keymanagementserviceshort}}.

Add a new resource to a supported cloud data service, and then select the root
key that you want to use for advanced encryption.

- To find out more about creating root keys with the
    {{site.data.keyword.keymanagementserviceshort}} service, see
    [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys).

- To find out more about bringing your own root keys to the
    {{site.data.keyword.keymanagementserviceshort}} service, see
    [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys).
