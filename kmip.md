---

copyright:
  years: 2024
lastupdated: "2024-03-25"

keywords: KMIP, VMWare, key protect

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
{:preview: .preview}
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Using the key management interoperability protocol (KMIP)
{: #kmip}

{{site.data.keyword.cloud_notm}}

To better facilitate the use of {{site.data.keyword.keymanagementservicefull}} keys to create key management interoperability protocol (KMIP) adapters for use with VMWare, {{site.data.keyword.keymanagementserviceshort}} now directly offers the ability to create adapters and upload certificates using the {{site.data.keyword.keymanagementserviceshort}} control plane (UI). 

This solution architecture describes the {{site.data.keyword.keymanagementserviceshort}} Native KMIP support for VMWare architecture for protecting your VMware® instances. Many storage encryption options are available to protect your VMware workload. {{site.data.keyword.keymanagementserviceshort}} Native KMIP support for VMWare works together with VMware native vSphere encryption and vSAN™ encryption. The vSphere and vSAN encryption provides simplified storage encryption management together with the security and flexibility of {{site.data.keyword.cloud}} {{site.data.keyword.keymanagementserviceshort}} customer-managed keys.

This solution is considered to be an extra component and extension of the [KMIP for VMWare](/docs/vmwaresolutions?topic=vmwaresolutions-kmip-overview) offering on {{site.data.keyword.cloud_notm}}. As a result, this document doesn't cover the existing configuration of these foundation solutions on {{site.data.keyword.cloud_notm}}. To understand more about the foundation solution architecture, see [Overview of VMware Solutions](/docs/vmwaresolutions?topic=vmwaresolutions-solution_overview).

This feature works in parallel with the current KMIP for VMWare solution. It is not currently possible to import adapters created using the VMWare solution into {{site.data.keyword.keymanagementserviceshort}} (or vice versa).
{: tip}

## Benefits
{: #kmip-overview-benefits}

While many storage encryption solutions are available for your VMware workload, {{site.data.keyword.keymanagementserviceshort}} Native KMIP support for VMWare offers the following benefits:

* Integration with VMware vSAN encryption and vSphere encryption, both of which are implemented in the hypervisor layer rather than the storage or virtual machine layer. This approach allows easy management and transparency to your storage solution and application.
* Fully managed key management server available in many {{site.data.keyword.cloud_notm}} multizone regions (MZRs).
* Integrating your VMware cluster with {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}} provides you with fully customer-managed keys that you can revoke at any time.


## Creating an adapter
{: #kmip-adapter-create}

A maximum of 200 adapters can be created on a single instance. Each adapter can have a maximum of 200 certificates associated with it.
{: important}

KMIP adapters are created using {{site.data.keyword.keymanagementserviceshort}} root keys. If you do not have a root key, [create one](/docs/key-protect?topic=key-protect-create-root-keys).

To create an adapter: 

1. Navigate to the **KMIP Adapters** panel using the left navigation. If this is your first adapter, the table should be empty. 

2. Find the **Create Adapter** button and click it.

3. In the open side panel, give the adapter a **Name** and, optionally, a **Description**. Note that names must contain at least two and no more than 40 characters. If you choose to give it a description, it must be at least two and no more than 240 characters. After that has been completed, you can choose the root key you want to use to create this adapter and to encrypt the KMIP keys it creates. Choosing a root key is mandatory. Note that if you delete this root key at a later date, the adapter no longer works.

4. Adding a public TLS certificate allows the holder of its corresponding private certificate to communicate with {{site.data.keyword.keymanagementserviceshort}} via its associated KMIP adapter. Only certificates authorized under a KMIP adapter will be permitted to make KMIP protocol requests against your instance. Resources managed via the KMIP protocol cannot be accessed via HTTP API.  
   While you do not need to add any certificates during the creation of the adapter, if you know which certificates you want to add, you can do so by clicking the **Add certificates (optional)** tab in the panel. In the resulting screen, click the **Upload certificate** button, give the certificate a name, and input the contents of the certificate (the material must be in PEM format and contain the `BEGIN CERTIFICATE` and `END CERTIFICATE` tags). Note that it can take a few minutes for the certificate to be associated. Also, a certificate can only be associated with a single adapter in a {{site.data.keyword.keymanagementserviceshort}} region.

Keep the private key of any uploaded certificates secure, as any certificate uploaded to a KMIP adapter will have the ability to make all supported KMIP operations.
{: important}

## Configuring a KMIP client to communicate with an adapter
{: #kmip-client}

To communicate with your adapter, you must either [setup VMWare](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.security.doc/GUID-70248689-A0E5-495B-A619-72561BA3A6C9.html){: external} (which will take care of your communications to your client, once you have associated the certificate provided by VMWare to it), or have created a KMIP client that can communicate over TCP with mTLS and can send messages using the TTLV message format [as described in KMIP specifications](https://docs.oasis-open.org/kmip/spec/v1.4/os/kmip-spec-v1.4-os.html#_Toc490660910){: external}.


## Granting access to KMIP 
{: #kmip-granting-access}

Review [roles and permissions](/docs/key-protect?topic=key-protect-manage-access) to learn how {{site.data.keyword.cloud_notm}} IAM roles map to {{site.data.keyword.keymanagementserviceshort}} actions.
{: tip}

The following IAM actions govern resources that will be used to manage access to KMIP resources:

- kms.kmip-management.create
- kms.kmip-management.list
- kms.kmip-management.read
- kms.kmip-management.delete

Each action grants the mentioned behavior to all `kmip_adapter` `certificate` and `kmip_object` resources in the instance, without granularity. 


## Viewing and updating adapter details
{: #kmip-adapter-view}

The adapter details panel allows you to learn about an adapter (for example, through its description) and also allows you to do actions like adding certificates.

To view the adapter details, click the ⋯ icon. This shows all of the details about the adapter. Here you can see its name, its description, any KMIP symmetric keys associated with the adapter, and certificates that have been uploaded to it. You can also use this panel to upload more certificates, as desired.

Each KMIP symmetric key that is created counts as a single key version and may incur a [charge, depending on the number of key versions you have](/docs/key-protect?topic=key-protect-pricing-plan).
{:important}

## KMIP supported objects and operations
{: #kmip-supported}

### KMIP supported operations
{: #kmip-supported-operations}

|  | Operation | Summary |
| - | - | - |
| 4.1 | Create | Creates a KMIP object
| 4.11 | Get | Retrieve object information, specifically the key material
| 4.12 | Get Attributes | Retrieve attribute metadata about the object
| 4.14 | Add Attribute | Add attribute metadata to the object
| 4.19 | Activate | Sets the object into an "Active" state. **The object may not be destroyed while the state is active.**
| 4.20 | Revoke | Sets the object into a "Compromised" state, if the revocation reason code given is "Key Compromise" or "CA Compromise". Otherwise sets the object into a "Deactivated" state.
| 4.21 | Destroy | Destroys the key material of the object. **This action cannot be reversed**
| 4.26 | Discover Versions | Requests supported KMIP protocol versions from the server. Only v1.4 will be returned.
| 4.9 | Locate | Search for objects matching the given criteria or attribute metadata.

### Supported objects
{: #kmip-supported-objects}

| | Object
| - | - |
| 2.2 | Symmetric Key