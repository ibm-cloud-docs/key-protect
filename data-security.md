---

copyright:
  years: 2017, 2025
lastupdated: "2025-05-27"

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

# Data security and compliance
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} has data security strategies in
place to meet your compliance needs and ensure that your data remains secure and
protected in the cloud.
{: shortdesc}

## Secrets management
{: #security-and-compliance-secrets}

Secrets required for deployment are managed with automation using a HashiCorp Vault and are not stored in charts, GitHub, or deployment scripts.

## Security readiness
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} ensures security readiness by
adhering to {{site.data.keyword.IBM_notm}} best practices for systems,
networking, and secure engineering.

To learn more about security controls across {{site.data.keyword.cloud_notm}},
see
[How do I know that my data is safe?](/docs/overview?topic=overview-security#security){: external}.
{: tip}

### Data encryption
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} uses [{{site.data.keyword.cloud_notm}} hardware security modules (HSMs)](https://www.ibm.com/products/hardware-security-module){: external} to generate provider-managed key material and perform [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption) operations. HSMs are tamper-resistant hardware devices that store and use cryptographic key material without exposing keys outside of a cryptographic boundary.

Access to {{site.data.keyword.keymanagementserviceshort}} takes place over HTTPS and uses Transport Layer Security (TLS) to encrypt data in transit.

Note that only the following TLS 1.2 and TLS 1.3 ciphers are supported:

- **TLS 1.2**:
    * `ECDHE-ECDSA-AES128-GCM-SHA256`
    * `ECDHE-ECDSA-AES256-GCM-SHA384`
    * `ECDHE-RSA-AES128-GCM-SHA256`
    * `ECDHE-RSA-AES256-GCM-SHA384`
    * `ECDHE-ECDSA-AES128-SHA256`
    * `ECDHE-ECDSA-AES256-SHA384`
    * `ECDHE-RSA-AES128-SHA256`
    * `ECDHE-RSA-AES256-SHA384`
- **TLS 1.3**:
    * `TLS_AES_256_GCM_SHA384`
    * `TLS_AES_128_GCM_SHA256`
    * `TLS_CHACHA20_POLY1305_SHA256`

If you attempt to use a cipher that is not on this list, you may experience connectivity issues. Update your client to use one of the supported ciphers. If you are using `openssl`, you can use the command `openssl ciphers -v` at the command line (or, for some installations of `openssl`, use the `-s -v` options) to show a verbose list of what ciphers your client supports.
{: tip}

### Data deletion
{: #data-deletion}

When you delete a key, the service marks the key as deleted, and the key transitions to the _Destroyed_ state. Keys in this state can no longer decrypt data that is associated with the key. Therefore, before you delete a key, review the data that is associated with the key and ensure that you no longer require access to it. Do not delete a key that is actively protecting data in your production environments.

If a key is deleted in error, it is possible to restore the key within 30 days of the key being deleted. After 30 days, the key can no longer be restored. For more information, check out [Restoring keys](/docs/key-protect?topic=key-protect-restore-keys).

Note that even if the key is not restored, your data remains in those services in its encrypted form. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database.

To help you determine what data is protected by a key, you can use {{site.data.keyword.keymanagementserviceshort}} APIs to [view associations between a key and your cloud resources](/docs/key-protect?topic=key-protect-view-protected-resources).
{: note}

### Account cancelation and data deletion
{: #account-cancelation}

Although instances that have non-deleted keys in them cannot be deleted, it is possible to close an [{{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account_settings) without first deleting all {{site.data.keyword.keymanagementserviceshort}} instances. If an account is closed and instances and keys belonging to it still exist, the instances and keys are permanently hard deleted.

For more information about closing an account, check out [Closing an account](/docs/account?topic=account-account-close).

## Compliance readiness
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} meets controls for global,
industry, and regional compliance standards, including GDPR, HIPAA, and ISO
27001/27017/27018, and others.

For a complete listing of {{site.data.keyword.cloud_notm}} compliance
certifications, see
[Compliance on the {{site.data.keyword.cloud_notm}}](https://ibm.com/cloud/compliance){: external}.
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
[Enabling EU support for your account](/docs/account?topic=account-eu-supported){: external}.

### General Data Protection Regulation (GDPR)
{: #gdpr}

The GDPR seeks to create a harmonized data protection law framework across the
EU. The regulation aims to give citizens back the control of their personal
data, and impose strict rules on any entity that hosts and processes that data.

{{site.data.keyword.IBM_notm}} is committed to providing clients and
{{site.data.keyword.IBM_notm}} Business Partners with innovative data privacy,
security, and governance solutions to assist them in their journey to GDPR
readiness.

To ensure GDPR compliance for your
{{site.data.keyword.keymanagementserviceshort}} resources,
[enable the EU supported setting](/docs/account?topic=account-eu-supported){: external}
for your {{site.data.keyword.cloud_notm}} account. You can learn more about how
{{site.data.keyword.keymanagementserviceshort}} processes and protects personal
data by reviewing the following addendums.

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://ibm.com/support/customer/csol/terms/?cat=dpa){: external}

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
the HIPAA Supported setting for your {{site.data.keyword.cloud_notm}} account.
To find out more, see
[Enabling the HIPAA Supported setting](/docs/account?topic=account-enabling-hipaa){: external}.

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} is ISO 27001, 27017, 27018
certified. You can view compliance certifications by visiting
[Compliance on the {{site.data.keyword.cloud_notm}}](https://ibm.com/cloud/compliance){: external}.

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
[Compliance on the {{site.data.keyword.cloud_notm}}](https://ibm.com/cloud/compliance){: external}.

### PCI DSS
{: #pci-dss}

{{site.data.keyword.keymanagementserviceshort}} meets controls for the Payment
Card Industry (PCI) data security standards to protect cardholder data. For
information about requesting an attestation of compliance, see
[Compliance on the {{site.data.keyword.cloud_notm}}](https://ibm.com/cloud/compliance){: external}
or contact an IBM representative.
