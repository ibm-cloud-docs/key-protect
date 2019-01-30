---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-30"

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

## Security and compliance
{: #security}

{{site.data.keyword.keymanagementserviceshort}} uses [{{site.data.keyword.cloud_notm}} hardware security modules (HSMs) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/hardware-security-module) to generate provider-managed key material and perform [envelope encryption](/docs/services/key-protect/envelope-encryption.html) operations. HSMs are tamper-resistant hardware devices that store and use cryptographic key material without exposing keys outside of a cryptographic boundary.

Access to the service takes place over HTTPS, and internally all {{site.data.keyword.keymanagementserviceshort}} communications use the Transport Layer Security (TLS) 1.2 protocol to encrypt data in transit.

To learn more about how {{site.data.keyword.keymanagementserviceshort}} meets your compliance requirements, check out [Platform and service compliance](/docs/overview/security.html#compliancetable).
{: tip}

### Can I store personal information as metadata for my keys?
{: #personal-data}

To protect the confidentiality of your personal data, do not store personally identifiable information (PII) as metadata for your keys. Personal information includes your name, address, phone number, email address, or other information that might identify, contact, or locate you, your customers, or anyone else.

You are responsible for ensuring the security of any information that you store as metadata for {{site.data.keyword.keymanagementserviceshort}} resources and encryption keys. For more examples of personal data, see section 2.2 of the [NIST Special Publication 800-122 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.

## Disaster recovery and data retention
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} has regional disaster recovery in place with a Recovery Time Objective (RTO) of one hour. The service follows {{site.data.keyword.cloud_notm}} requirements for planning and recovering from disaster events. For more information, see [Disaster recovery](/docs/overview/zero_downtime.html#disaster-recovery).

### Does {{site.data.keyword.keymanagementserviceshort}} retain data after a resource is deleted?
{: #data-retention}

Deleted keys transition to the [_Destroyed_ state](/docs/services/key-protect/concepts/key-states.html). The service shreds the key material from its primary database and then updates the metadata for the key to indicate that the resource was deleted. To access data that was previously deleted, {{site.data.keyword.keymanagementserviceshort}} must initiate the [{{site.data.keyword.cloud_notm}} disaster recovery](/docs/overview/zero_downtime.html#disaster-recovery) process. All data that is required for disaster recovery is encrypted and cannot be accessed by using normal procedures. 

Remember to assess the type of data that your root key is protecting in the cloud. If needed, use an on-site hardware security module or key management system to back up your root key material securely.

### What happens when I need to deprovision my service instance?
{: #deprovision-service}

Goodbyes can be difficult. If you decide to move on from {{site.data.keyword.keymanagementserviceshort}}, you must delete any remaining keys from your service instance before you can deprovision the service. After you delete your service instance, {{site.data.keyword.keymanagementserviceshort}} uses [envelope encryption](/docs/services/key-protect/concepts/envelope-encryption.html) to crypto-shred any data that is associated with the service instance. 
