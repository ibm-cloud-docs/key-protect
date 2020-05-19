---

copyright:
  years: 2020
lastupdated: "2020-04-03"

keywords: public isolation for Key Protect, compute isolation for Key Protect, Key Protect architecture, workload isolation in Key Protect

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:term: .term}

# Architecture and workload isolation
{: #compute-isolation}

Review the sample architecture and compute isolation characteristics for
{{site.data.keyword.keymanagementservicelong}}.
{: shortdesc}

## {{site.data.keyword.keymanagementserviceshort}} architecture
{: #architecture}

The following architecture diagram shows how {{site.data.keyword.keymanagementserviceshort}}
components work to protect your sensitive data and keys.

![The diagram shows how {{site.data.keyword.keymanagementserviceshort}} components protect sensitive data and keys.](images/kp-architecture.svg)
{: caption="Figure 1. {{site.data.keyword.keymanagementserviceshort}} architecture" caption-side="bottom"}

Access to the {{site.data.keyword.keymanagementserviceshort}} service takes
place over HTTPS. All communication uses the Transport Layer Security (TLS) 1.2
protocol to encrypt data in transit.
{: note}

| Components | Description |
| --- | --- |
| {{site.data.keyword.keymanagementserviceshort}} REST API | The {{site.data.keyword.keymanagementserviceshort}} REST API drives encryption key creation and management across {{site.data.keyword.cloud_notm}} services.|
| IBM-managed hardware security module | Behind the scenes, {{site.data.keyword.cloud_notm}} data centers provide the hardware to protect your keys. Hardware security modules (HSMs) are tamper-resistant hardware devices that store and use cryptographic key material without exposing keys outside of a cryptographic boundary. All cryptographic operations, such as key creation and key rotation, are performed within the HSM. |
| Customer-managed encryption keys | Root keys are symmetric key-wrapping keys that protect data encryption keys with [envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption). Root keys never leave the boundary of the hardware security module. |
| Dedicated key storage | Key metadata is stored in highly durable, dedicated storage for {{site.data.keyword.keymanagementserviceshort}} that is encrypted at rest with additional application layer encryption. |
{: caption="Table 1. {{site.data.keyword.keymanagementserviceshort}} service components" caption-side="top"}

## {{site.data.keyword.keymanagementserviceshort}} workload isolation
{: #workload-isolation}

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional
service that supports the following workload isolation characteristics:

- {{site.data.keyword.keymanagementserviceshort}} resources are isolated within
a secure runtime environment that is shared across an
{{site.data.keyword.cloud_notm}} multi-zone region.
- Multiple instances of the {{site.data.keyword.keymanagementserviceshort}}
service run on shared clusters and a shared container.
- To isolate workloads, {{site.data.keyword.keymanagementserviceshort}} processes
API requests within a security context that is user-based and unique to each customer.
