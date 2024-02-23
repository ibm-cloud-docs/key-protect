---

copyright:
  years: 2024
lastupdated: "2024-02-23"

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

# Deploying a hardware security module (HSM) to use with Key Protect on Satellite
{: #satellite-hsm-deploy}

{{site.data.keyword.keymanagementserviceshort}} on Satellite must connect to two on-prem customer-managed hardware security modules (HSMs), which is the root of trust store for master encryption keys and provides the FIPS certified cryptographic boundary for key operations performed by {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

To work correctly with {{site.data.keyword.keymanagementserviceshort}}, these HSMs must not only be of a specific type, but must be configured to work specifically with {{site.data.keyword.keymanagementserviceshort}}.

## Overview
{: #satellite-hsm-overview}

Before a {{site.data.keyword.keymanagementserviceshort}} on Satellite location can be deployed, HSMs must both exist and have been configured to create the kinds of keys that work with {{site.data.keyword.keymanagementserviceshort}}. At a high level, this is a two step process:

* **The purchase and initial setup of the HSMs**. Users are responsible to purchase and setup the HSMs within their own infrastructure. For more information about the type of HSMs that are supported, check out [About the HSMs supported for use with {{site.data.keyword.keymanagementserviceshort}} on Satellite](#satellite-hsm-supported). Because these HSMs are not purchased from {{site.data.keyword.IBM_notm}}, make sure to follow the relevant product documentation from your HSM vendor in regard to setup, maintenance, and resolving service incidents. {{site.data.keyword.IBM_notm}} is only responsible for errors that result from issues with the {{site.data.keyword.keymanagementserviceshort}} service, not from any issues with your HSM.
* **Configure the HSMs to work with {{site.data.keyword.keymanagementserviceshort}}**. In order for an HSM to work properly with {{site.data.keyword.keymanagementserviceshort}}, it must not only have a [partition](#satellite-hsm-supported-partitions) dedicated to work with {{site.data.keyword.keymanagementserviceshort}}, but it also must be configured to produce keys using the key algorithms supported by {{site.data.keyword.keymanagementserviceshort}}. The labels for these keys must be provided as part of deploying the [{{site.data.keyword.keymanagementserviceshort}} console](/docs/key-protect?topic=key-protect-satellite-ui-deploy#satellite-ui-deploy-before-begin).

## About the HSMs supported for use with {{site.data.keyword.keymanagementserviceshort}} on Satellite
{: #satellite-hsm-supported}

{{site.data.keyword.keymanagementserviceshort}} requires particular HSMs running specific firmware and software versions to run properly. Specifically, {{site.data.keyword.keymanagementserviceshort}} supports:

* [Thales SafeNet Luna Network](https://thalesdocs.com/gphsm/luna/7.4/docs/network/Content/PDF_Network/Product%20Overview.pdf) A730 and A750 model HSMs. The A750 is an enterprise-grade appliance, able to handle up to 10000 AES-GCM / ECC P256 concurrent transactions per second (TPS) or up to 5000 RSA-2048 TPS. These HSMs are high efficient and powerful appliances designed to provide mainly over the network cryptographic services. To do so, they always provide four physical adapters.
* Application/OS version 7.4.2.
* Firmware version 7.3.3.

Older firmware and application/OS version are not explicitly supported as using them runs the risk of having issues if {{site.data.keyword.keymanagementserviceshort}} adds features that depend on later hardware or software versions. For this reason, **make sure to keep your firmware and software versions as up to date as possible**.

Depending on the version of the software application and supported FIPS 140-2 Level 3 firmware, the upgrade path through prior versions may be required. The Thales HSM documentation prescribes the correct upgrade path. As of this writing, the path to 7.3.1 is through 7.3.0. In other words, update to version 7.3.0 before updating to version 7.3.1.
{: tip}

* For more information about Thales SafeNet Luna Network HSMs releases, check out [Thales SafeNet Luna Network HSMs Available Documentation Releases](https://thalesdocs.com/gphsm/luna/7/docs/network/Content/Home_Luna.htm).

* For details about the certifications, check out [FIPS 140-2 Certificate](https://csrc.nist.gov/Projects/Cryptographic-Module-Validation-Program/Certificate/3205).

For security best practice, your HSMs should run in FIPS mode. This allows your HSM to create FIPS-compliant keys. To learn how check the current policy setting, check out [hsm showpolicies](https://thalesdocs.com/gphsm/luna/7/docs/network/Content/lunash/commands/hsm/hsm_showpolicies.htm). `Enable non-FIPS algorithms` should be `Disallowed`.
{: tip}

The brochure for Thales Luna HSMs can be found [here](https://cpl.thalesgroup.com/encryption/hardware-security-modules/network-hsms).

### Network connectivity between {{site.data.keyword.keymanagementserviceshort}} and HSM (recommended) 
{: #satellite-hsm-supported-interface-bonding} 

The best practice network configuration is to use 10Gb links for cryptographic traffic between {{site.data.keyword.keymanagementserviceshort}} and your HSMs and a bond of 2Gb links for management and administration traffic. {{site.data.keyword.keymanagementserviceshort}} requires TCP connectivity to HSM on port 1792 for NTLS protocol. To check the connectivity, issue a `netcat` command on the worker nodes which would be assigned to {{site.data.keyword.keymanagementserviceshort}}:

`nc -vz <HSM-ipaddr> 1792`

Where `HSM-ipaddr` is the IP address of your HSM.

Bonding provides standby fault tolerance reliability, but does not provide load balancing.
{: tip}

### Partitions and partition types
{: #satellite-hsm-supported-partitions}

If you have new or factory-reset HSMs, you must create an application partition to work with the {{site.data.keyword.keymanagementserviceshort}} service. [Follow the instructions in your HSM provider](https://thalesdocs.com/gphsm/luna/7/docs/network/Content/admin_partition/Preface.htm){: external} to create a partition, keeping in mind that your **partition label and partition crypto officer password must be the same for the partition in both HSMs** that will be connected to {{site.data.keyword.keymanagementserviceshort}}. This is because the partition crypto officer password is consumed by {{site.data.keyword.keymanagementserviceshort}} as part of the PKCS#11 session API login process.

The Thales SafeNet Luna Network HSM uses two types of partitions:
 
* **Administrative partitions**: This is where HSM-wide policies are set and changed, application partitions are created or destroyed, HSM firmware and capabilities are updated, and so on.
* **Application partition**: This is where cryptographic operations are performed by user applications.
 
While it is only possible to have only one administrative partition in a given HSM, it is possible to have multiple application partitions to address multi-tenancy configurations. This is achieved by partitioning the key storage space into multiple application partitions, with the ability to set the size of each partition. A user of a tenant has no way to see the other application partitions. Each application partition is also encrypted using partition-specific encryption keys.

The A750 appliance supports by default the creation of up to five application partitions, and therefore the management of up to five different tenants. This can be increased for the A750 model up to 20 partitions by purchasing additional partition licenses.

## Configuring your HSM to produce the keys {{site.data.keyword.keymanagementserviceshort}} needs
{: #satellite-hsm-configure}

For {{site.data.keyword.keymanagementserviceshort}} to work with an HSM, several different keys must be created on the HSM and stored as persistent token objects in the partition memory. Each of these keys must be given a label, and these labels must be provided when deploying {{site.data.keyword.keymanagementserviceshort}} on Satellite. These keys are:

| Key type                                 | Key role                                                                                                                                          |
|------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Master Key Encryption Key** (MKEK)     | (256bit AES key) A root level encryption key for wrapping and unwrapping instance keys in {{site.data.keyword.keymanagementserviceshort}}.        |
| **Signing key** (SKEY)                   | (256bitsAES key) Used for signing and verification of instance and user keys in {{site.data.keyword.keymanagementserviceshort}}.                  |
| **Import Key** (IKEY)                    | (192bit DES3 key) Used to encrypt and unwrap the user's key materials be imported into {{site.data.keyword.keymanagementserviceshort}}.           |
| **Transit Key Encryption Keys** (TKEKs)  | 10 pairs of RSA asymmetric key pairs, which are used to securely Bring Your Own Keys (BYOK) into {{site.data.keyword.keymanagementserviceshort}}. |

The specific parameters that must be set to produce the correct keys can be acquired by opening an {{site.data.keyword.cloud_notm}} service ticket indicating your desire to purchase {{site.data.keyword.keymanagementserviceshort}} on Satellite. A {{site.data.keyword.keymanagementserviceshort}} representative will contact you and initiate the process for you to acquire the necessary configuration information.

Many different tools can be used to create these keys. Consult with Thales technical support to find a secure way to create these keys based on your organization's security policy and compliance requirements.
{: tip}

## Gather the information needed to connect the HSM with the {{site.data.keyword.keymanagementserviceshort}} console
{: #satellite-hsm-ui-values}

In order to [deploy {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-satellite-ui-deploy), you must provide the information to connect to the HSM and to perform the kinds of privileged actions on the HSM that are necessary for {{site.data.keyword.keymanagementserviceshort}} to function as part of your interactions with your service representative.

You must provide this information for both HSMs.
{: important}

* **HSM IP address**: The IP address of your HSM. This is needed in order to connect to the worker nodes that you assigned to {{site.data.keyword.keymanagementserviceshort}}.
* **HSM server certificate**: The NTLS communications used by the Thales HSM requires certificate exchanges between the HSM and {{site.data.keyword.keymanagementserviceshort}}. You must create a TLS certificate in your HSM and provide the certificate {{site.data.keyword.keymanagementserviceshort}} will use to verify communications from the HSM.
* **Partition label**: The name of the partition that you created for {{site.data.keyword.keymanagementserviceshort}} to use.
* **Partition crypto officer password**: The credential {{site.data.keyword.keymanagementserviceshort}} needs to login to the relevant partition on the HSM to perform key operations.
* **Master key label**: {{site.data.keyword.keymanagementserviceshort}} uses a Master Key Encryption Key in your partition. A label or name is assigned to this key and is used by {{site.data.keyword.keymanagementserviceshort}} in PKCS#11 API to refer to the master key.
* **Signing key label**: A label or name for a key in a partition, which is used for data authentication such as data signing and verification.
* **Import key label**: {{site.data.keyword.keymanagementserviceshort}} supports importing Bring Your Own Key (BYOK) by a DES3 encryption Key. A label or name is assigned to this key.
* **Secure import key label prefix (TKEK)**: {{site.data.keyword.keymanagementserviceshort}} supports a secure mechanism to Bring Your Own Key (BYOK) into an HSM. The prefix used by the HSM for these keys must be known to {{site.data.keyword.keymanagementserviceshort}}.
* **Activity Tracker ingestion key**: To receive audit logs, you must create an {{site.data.keyword.at_full_notm}} instance and an ingestion key.

There are two additional credentials you must share before your service can be deployed, the **HSM client cert** and the **HSM client key**. These credentials enable NTLS communications used by the Thales HSM for exchanges between the HSM and {{site.data.keyword.keymanagementserviceshort}} running on your worker node. You must create and register with the HSM a TLS certificate for the worker node (client) that will connect to the HSM and provide the client certificate and key that {{site.data.keyword.keymanagementserviceshort}} uses to communicate with the HSM. The exact instructions to share these credentials are communicated as part of your conversations with your service representative.

Make a note of these values for both HSMs before attempting to [deploy {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-satellite-ui-deploy).
