---

copyright:
  years: 2023
lastupdated: "2026-03-19"

keywords: key management in IBM Cloud, differences between Secrets Manager and Key Protect, when to use Secrets Manager, HPCS, Key Protect use cases, single tenant, multi-tenant

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}


# Which data security service is best for me?
{: #manage-secrets-ibm-cloud}

With {{site.data.keyword.cloud_notm}}, you can choose from various secrets management and data protection offerings that help you to protect your sensitive data and centralize your secrets.
{: shortdesc}

## Use cases
{: #which-data-protection-service}

The following table lists the different offerings that you can use with {{site.data.keyword.cloud_notm}} to protect your data.

| Scenario | What to use |
| --- | --- |
| You need to create and manage encryption keys that are backed by FIPS 140-2 Level 3 or FIPS 140-3 Level 4 hardware. | You can use **{{site.data.keyword.keymanagementserviceshort}}** <prod-st>or HPCS</prod-st> to generate and import encryption keys by either using a multi-tenant service with shared hardware (featuring FIPS 140-2 Level 3 certification), or dedicated hardware using hardware that has been submitted to NIST for FIPS 140-3 Level 4 certification. |
| As a DevOps team contributor, you need to create, lease, and manage API keys, credentials, database configurations, and other secrets for your services and applications. | With **[{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager)**, you can manage secrets of various types in a dedicated instance. |
| You need to generate, renew, and manage SSL/TLS certificates for your deployments. | You can also manage your SSL/TLS certificates and private keys in dedicated instance of **[{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager)**. |
{: caption="Secrets management and data protection scenarios" caption-side="top"}

## What are key features for each data protection service?
{: #key-features}

As you plan your data protection strategy, some differences between services to consider include the level of data isolation that your workload requires.

For a higher level of security and control, your business might benefit from the data isolation that a single-tenant offering provides, such as {{site.data.keyword.secrets-manager_short}} or {{site.data.keyword.hscrypto}}. You might also decide that the reduced cost and scalability benefits of a multi-tenant service, such as {{site.data.keyword.keymanagementserviceshort}}, are better suited to your needs. The following table lists key features for each service.


|                                         | **Standard {{site.data.keyword.keymanagementserviceshort}}** | **{{site.data.keyword.secrets-manager_short}}**   | **Dedicated {{site.data.keyword.keymanagementserviceshort}}**                |
|-----------------------------------------|-----------------------------------------------------|---------------------------------------------------|---------------------------------------------------|
| Secret types                            | Symmetric encryption keys                           | Arbitrary secrets  \n IAM credentials  \n Key-value secrets  \n SSL/TLS certificates  \n User credentials | Symmetric encryption keys                         |
| Multi-tenant[^multi-tenant]             | ![Checkmark icon](../../icons/checkmark-icon.svg)   |                                                   |                                                   |
| Single-tenant[^single-tenant]           |                                                     | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| HSM backed[^hsm]                        | ![Checkmark icon](../../icons/checkmark-icon.svg)   |                                                   | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Runs in secure enclave[^secure-enclave] |                                                     |                                                   | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Client initialised and controlled HSM   |                                                     |                                                   | ![Checkmark icon](../../icons/checkmark-icon.svg) |
{: caption="Key features for {{site.data.keyword.cloud_notm}} data protection services" caption-side="top"}
{: summary="The table compares features across {{site.data.keyword.secrets-manager_short}}, {{site.data.keyword.cloudcerts_short}}, {{site.data.keyword.keymanagementserviceshort}}, and {{site.data.keyword.hscrypto}}. The first column lists features. The first row features the names of the services in the table, followed by a row listing the types of secrets that are supported by each service. The third row uses checkmarks to indicate whether a service is multi-tenant. The fourth row uses checkmarks to indicate whether a service is single-tenant. The fifth row uses checkmarks to indicate whether a service is backed by a hardware security module (HSM). The sixth row uses checkmarks to indicate whether the service runs in a secure enclave. The seventh row uses checkmarks to indicate whether the service uses a client initialized and controlled HSM."}
{: class="comparison-table"}


[^multi-tenant]: A multi-tenant service uses a single instance of its software (and its underlying database and hardware) to serve multiple tenants. [Learn more](https://www.ibm.com/think/topics/multi-tenant){: external}.

[^single-tenant]: A single-tenant service creates a dedicated instance of its software (and its underlying database and hardware) for each individual tenant.

[^hsm]: A service that is backed by a hardware security module (HSM) uses tamper-resistant, FIPS-validated physical hardware as its root of trust for cryptographic storage and processing of encryption keys.

[^secure-enclave]: Mitigates internal as well as external attack vectors to gain unauthorised access to keys.

## Can these services work together?
{: #service-comparison-da}

Yes. For many use cases, it is important to use more than one service to completely sercure your data. For more information about the deployable architecture that covers security services, check out [What is cloud security?](/docs/security-hub?topic=security-hub-cloud-security)

## How do I get started?
{: #get-started-data-protection}

Each service supports either a Lite plan or a free trial that you can use to try its service capabilities for free. Get started by creating an instance of a service from the {{site.data.keyword.cloud_notm}} catalog.

- [{{site.data.keyword.keymanagementserviceshort}}](/catalog/services/key-protect){: external}
- [{{site.data.keyword.hscrypto}}](/catalog/services/hs-crypto){: external}
- [{{site.data.keyword.secrets-manager_short}}](/catalog/services/secrets-manager){: external}
