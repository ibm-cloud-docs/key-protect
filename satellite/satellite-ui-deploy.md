---

copyright:
  years: 2023
lastupdated: "2023-11-06"

keywords: satellite, ui, deploy

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

# Deploying the Key Protect console to Satellite
{: #satellite-ui-deploy}

After you have [successfully deployed your Satellite location](/docs/satellite?topic=satellite-getting-started), you can initiate a creation request for {{site.data.keyword.keymanagementservicefull}} on Satellite, followed by creating a service ticket to complete the installation.
{: shortdesc}

To ensure that {{site.data.keyword.keymanagementserviceshort}} is installed in your Satellite location correctly, your configuration parameters, including your Hardware Security Module (HSM) information, are shared out-of-band during your interaction with your service representative.
{: important}

Note that when you click **Create** on the catalog page, you initiate a **creation request**. This request must be followed by an interaction with a service representative (if you have not already initiated this interaction). If you do not contact a service representative, your creation request cannot succeed. While you can intitiate a creation request without first having configured your HSMs, you cannot initiate the request without first having created a [Satellite location](/docs/satellite?topic=satellite-getting-started). The best practice, however, is to begin your interaction with {{site.data.keyword.IBM_notm}} well in advance of your attempt to initiate a creation request.

While the {{site.data.keyword.IBM_notm}} Console is used to create the {{site.data.keyword.keymanagementserviceshort}} service on Satellite, the console itself cannot currently be used to access the {{site.data.keyword.keymanagementserviceshort}} APIs that are used to create keys or perform other key actions (such as rotating keys, deleting keys, editing keys, and so on). Those key actions must be performed through direct calls to the [{{site.data.keyword.keymanagementserviceshort}} APIs](/apidocs/key-protect) or by using the CLI.
{: note}

## Before you begin
{: #satellite-ui-deploy-before-begin}

Before {{site.data.keyword.keymanagementserviceshort}} on Satellite can be successfully deployed, you must have created a Satellite location and have both deployed and [correctly configured](/docs/key-protect?topic=key-protect-satellite-hsm-deploy) at least two HSMs. You must also have [gathered the information about the HSM that Satellite must consume](/docs/key-protect?topic=key-protect-satellite-hsm-deploy#satellite-hsm-ui-values) and have [deployed the {{site.data.keyword.cloud_notm}} Databases service in your Satellite location](docs/cloud-databases?topic=cloud-databases-satellite-on-prem).

For the smoothest interaction with your service representative, the best practice is to gather these configuration variables before initiating a creation request for {{site.data.keyword.keymanagementserviceshort}} on Satellite.

* **HSM IP address**: The IP address of your HSM. This is needed in order to connect to the worker nodes that you assigned to {{site.data.keyword.keymanagementserviceshort}}.
* **HSM server certificate**: The NTLS communications used by the Thales HSM requires certificate exchanges between the HSM and {{site.data.keyword.keymanagementserviceshort}}. You must create a TLS certificate in your HSM and provide the certificate {{site.data.keyword.keymanagementserviceshort}} will use to verify communications from the HSM.
* **Partition label**: The name of the partition that you created for {{site.data.keyword.keymanagementserviceshort}} to use.
* **Partition crypto officer password**: The credential {{site.data.keyword.keymanagementserviceshort}} needs to login to the relevant partition on the HSM to perform key operations.
* **Master key label**: {{site.data.keyword.keymanagementserviceshort}} uses a Master Key Encryption Key in your partition. A label or name is assigned to this key and is used by {{site.data.keyword.keymanagementserviceshort}} in PKCS#11 API to refer to the master key.
* **Signing key label**: A label or name for a key in a partition, which is used for data authentication such as data signing and verification.
* **Import key label**: {{site.data.keyword.keymanagementserviceshort}} supports importing Bring Your Own Key (BYOK) by a DES3 encryption Key. A label or name is assigned to this key.
* **Secure import key label prefix (TKEK)**: {{site.data.keyword.keymanagementserviceshort}} supports a secure mechanism to Bring Your Own Key (BYOK) into an HSM. The prefix used by the HSM for these keys must be known to {{site.data.keyword.keymanagementserviceshort}}.
* **Activity Tracker ingestions key**: To receive audit logs, you must create an {{site.data.keyword.at_full_notm}} instance and an ingestion key.

There are two additional credentials you must share before your service can be deployed, the **HSM client cert** and the **HSM client key**, but these are not shared as part of deploying the UI itself. These credentials enable NTLS communications used by the Thales HSM for exchanges between the HSM and {{site.data.keyword.keymanagementserviceshort}} running on your worker node. You must create and register with the HSM a TLS certificate for the worker node (client) that will connect to the HSM and provide the client certificate and key that {{site.data.keyword.keymanagementserviceshort}} uses to communicate with the HSM. The exact instructions to share these credentials are communicated as part of your conversations with your service representative.
{: tip}

## Initiating a {{site.data.keyword.keymanagementserviceshort}} on Satellite creation request
{: #satellite-ui-deploy-catalog}

To provision the {{site.data.keyword.keymanagementserviceshort}} service, complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](/login/){: external}.

2. Click **Catalog** to view the list of services that are available on {{site.data.keyword.cloud_notm}}.

3. Search for "Key Protect" in the ***Search the catalog...*** field and click `Key Protect`.

4. On the {{site.data.keyword.keymanagementserviceshort}} catalog page, select **Satellite**.

4. Note the **Before you begin** checklist and confirm you have completed the required steps.

5. Select the **Key quota** you want assigned to this location. This number represents the maximum number of keys which can be created in this location. This number can be changed later by opening a service ticket. Note that the quota can only be set in groups of 100 keys. For more information, check out [Pricing for {{site.data.keyword.keymanagementserviceshort}} on Satellite](/docs/key-protect?topic=key-protect-pricing-plan-satellite).

6. In the **Configure your resource** section:
    * Give the service a **Name**. While not necessary, it is a best practice to choose a name relevant to the usage you plan for the service.
    * Select a **Resource group**. By default the `Default` resource group will be chosen.
    * (Optional) Give the service a **Tag** (for example, `test`) or an **Access management tag**, which can help categorize the service.

7. Configure the HSMs the service will connect to. **Note that you must configure two HSMs**. To configure these HSMs, provide the HSM information you should have gathered [before you begin](#satellite-ui-deploy-before-begin).

8. After you have checked and double checked your HSM configuration information, click **Create** to provision {{site.data.keyword.keymanagementserviceshort}}. Note that the process of creating the database for the service can take more than an hour.

## Using the {{site.data.keyword.keymanagementserviceshort}} service
{: #satellite-ui-deploy-use}

Now that {{site.data.keyword.keymanagementserviceshort}} on Satellite has been successfully deployed, you're ready to use {{site.data.keyword.keymanagementserviceshort}} to encrypt your data at rest, which you can do using the the {{site.data.keyword.keymanagementserviceshort}} endpoint through the APIs.

* For information about how envelope encryption works, check out [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

* To learn how to create root keys, check out [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys).

* If you added a root key to the service, learn more about using the root key to protect the keys that encrypt your data at rest by checking out [Wrapping keys](/docs/key-protect?topic=key-protect-wrap-keys).

* To find out more about authorizing other cloud services to integrate with {{site.data.keyword.keymanagementserviceshort}}, check out the [Integrations](/docs/key-protect?topic=key-protect-integrate-services) documentation.

* To find out more about programmatically managing your keys, check out the [{{site.data.keyword.keymanagementserviceshort}} API reference](/apidocs/key-protect){: external} documentation.
