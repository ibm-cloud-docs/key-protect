---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Security and compliance
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} has data security strategies in
place to meet your compliance needs and ensure that your data remains secure and
protected in the cloud.
{: shortdesc}

## Security readiness
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} ensures security readiness by
adhering to {{site.data.keyword.IBM_notm}} best practices for systems,
networking, and secure engineering.

To learn more about security controls across {{site.data.keyword.cloud_notm}},
see
[How do I know that my data is safe?](/docs/overview?topic=overview-security#security).
{: tip}

### Data encryption
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} uses
[{{site.data.keyword.cloud_notm}} hardware security modules (HSMs)](https://www.ibm.com/cloud/hardware-security-module){: external}
to generate provider-managed key material and perform
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)
operations. HSMs are tamper-resistant hardware devices that store and use
cryptographic key material without exposing keys outside of a cryptographic
boundary.

Access to the service takes place over HTTPS, and internal
{{site.data.keyword.keymanagementserviceshort}} communication uses the Transport
Layer Security (TLS) 1.2 protocol to encrypt data in transit.

### Data deletion
{: #data-deletion}

When you delete a key, the service marks the key as deleted, and the key
transitions to the _Destroyed_ state. Keys in this state are no longer
recoverable, and the cloud services that use the key can no longer decrypt data
that is associated with the key. Your data remains in those services in its
encrypted form. Metadata that is associated with a key, such as the key's
transition history and name, is kept in the
{{site.data.keyword.keymanagementserviceshort}} database.

Deleting a key in {{site.data.keyword.keymanagementserviceshort}} is a
destructive operation. Keep in mind that after you delete a key, the action
cannot be reversed, and any data that is associated with the key is immediately
lost at the moment the key is deleted. Before you delete a key, review the data
that is associated with the key and ensure that you no longer require access to
it. Do not delete a key that is actively protecting data in your production
environments.

To help you determine what data is protected by a key, you can use
{{site.data.keyword.keymanagementserviceshort}} APIs to
[view associations between a key and your cloud resources](/docs/key-protect?topic=key-protect-view-protected-resources).
{: note}

## Compliance readiness
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} meets controls for global,
industry, and regional compliance standards, including GDPR, HIPAA, and ISO
27001/27017/27018, and others.

For a complete listing of {{site.data.keyword.cloud_notm}} compliance
certifications, see
[Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### EU support
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} has extra controls in place to
protect your {{site.data.keyword.keymanagementserviceshort}} resources in the
European Union (EU).

If you use {{site.data.keyword.keymanagementserviceshort}} resources in the
Frankfurt, Germany region to process personal data for European citizens, you
can enable the EU Supported setting for your {{site.data.keyword.cloud_notm}}
account. To find out more, see
[Enabling the EU Supported setting](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)
and
[Requesting support for resources in the European Union](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### General Data Protection Regulation (GDPR)
{: #gdpr}

The GDPR seeks to create a harmonized data protection law framework across the
EU. The regulation aims to give citizens back the control of their personal
data, and impose strict rules on any entity that hosts and processes that data.

{{site.data.keyword.IBM_notm}} is committed to providing clients and
{{site.data.keyword.IBM_notm}} Business Partners with innovative data privacy,
security, and governance solutions to assist them in their journey to GDPR
readiness.

To ensure GDPR compliance for your {{site.data.keyword.keymanagementserviceshort}} resources,
[enable the EU supported setting](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported)
for your {{site.data.keyword.cloud_notm}} account. You can learn more about how
{{site.data.keyword.keymanagementserviceshort}} processes and protects personal
data by reviewing the following addendums.

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA support
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} does not process, store,
transmit, or otherwise interface with personal health information (PHI).
Therefore, the service can be integrated with any HIPAA offering without impact
to its HIPAA readiness. As such, you can use
{{site.data.keyword.keymanagementserviceshort}} to generate and manage keys for
HIPAA ready applications. Those keys are protected by the
{{site.data.keyword.keymanagementserviceshort}} trust anchor, which is backed by
a hardware security module (HSM) that is tamper-resistant and FIPS-140-2 level 3
certified.

If you or your company is a covered entity as defined by HIPAA, you can enable
the HIPPA Supported setting for your {{site.data.keyword.cloud_notm}} account.
To find out more, see
[Enabling the HIPAA Supported setting](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} is ISO 27001, 27017, 27018
certified. You can view compliance certifications by visiting
[Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

### Service Organization Controls (SOC)
{: #soc}

{{site.data.keyword.keymanagementserviceshort}} meets Service Organization
Control (SOC) compliance for the following types:

- SOC 1 Type 2
- SOC 2 Type 1
- SOC 2 Type 2
- SOC 3

For information about requesting an {{site.data.keyword.cloud_notm}} SOC report,
see
[Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

### PCI DSS
{: #pci-dss}

{{site.data.keyword.keymanagementserviceshort}} meets controls for the Payment
Card Industry (PCI) data security standards to protect cardholder data. For
information about requesting an attestation of compliance, see
[Compliance on the {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}
or contact an IBM representative.
