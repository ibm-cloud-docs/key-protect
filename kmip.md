---

copyright:
  years: 2026
lastupdated: "2026-06-24"

keywords: KMIP, VMware, key protect, key management interoperability protocol

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Using the key management interoperability protocol (KMIP)
{: #kmip}

{{site.data.keyword.keymanagementservicefull}} provides native support for the key management interoperability protocol (KMIP), allowing you to create KMIP adapters and upload certificates directly through the {{site.data.keyword.keymanagementserviceshort}} console.
{: shortdesc}

This solution describes the {{site.data.keyword.keymanagementserviceshort}} native KMIP support architecture for protecting your VMware® instances. {{site.data.keyword.keymanagementserviceshort}} native KMIP support works with VMware native vSphere encryption and vSAN™ encryption to provide simplified storage encryption management with the security and flexibility of {{site.data.keyword.cloud}} {{site.data.keyword.keymanagementserviceshort}} customer-managed keys.

This solution is an alternative to the [KMIP for VMware](/docs/vmwaresolutions?topic=vmwaresolutions-kmip-overview){: external} offering on {{site.data.keyword.cloud_notm}}. This document does not cover the configuration of these foundation solutions. For more information about the foundation solution architecture, see [Overview of VMware Solutions](/docs/vmwaresolutions?topic=vmwaresolutions-solution_overview){: external}.

This feature works in parallel with the current KMIP for VMware solution. You cannot import adapters created with the VMware solution into {{site.data.keyword.keymanagementserviceshort}}, or vice versa.
{: tip}

## Benefits
{: #kmip-overview-benefits}

{{site.data.keyword.keymanagementserviceshort}} native KMIP support offers the following benefits:

**VMware certification**
:   KMIP support in {{site.data.keyword.keymanagementserviceshort}} is [certified by VMware](https://compatibilityguide.broadcom.com/detail?program=kms&productId=60700&persona=live){: external} and can be directly integrated with any service or platform that accepts encryption through a KMIP KMS server. KMIP support is integrated and managed by {{site.data.keyword.keymanagementserviceshort}}, eliminating the need for third-party KMIP server support.

**Hypervisor-level encryption**
:   Integration with VMware vSAN encryption and vSphere encryption provides encryption at the hypervisor layer rather than the storage or virtual machine layer. This approach simplifies management and provides transparency to your storage solution and application.

**Fully managed service**
:   Key management server is fully managed and available in many {{site.data.keyword.cloud_notm}} multizone regions (MZRs).

**Customer-managed keys**
:   You maintain full control over your encryption keys and can revoke them at any time.

**Cost-effective**
:   KMIP symmetric keys are [charged as a single key version](/docs/key-protect?topic=key-protect-pricing-plan), so you only pay for what you use.

## Creating an adapter
{: #kmip-adapter-create}
{: ui}

A maximum of 200 adapters can be created on a single instance. Each adapter can have a maximum of 200 certificates associated with it.
{: important}

KMIP adapters are created using {{site.data.keyword.keymanagementserviceshort}} root keys. If you do not have a root key, [create one](/docs/key-protect?topic=key-protect-create-root-keys).

Before you begin, ensure that you have the [`Manager` role or the `KmipAdapterManager` role](/docs/key-protect?topic=key-protect-manage-access) on the instance.

To create an adapter:

1. In the navigation menu, click **KMIP adapters**. If this is your first adapter, the table is empty.

2. Click **Create**.

3. In the side panel, provide the following information:
   * **Name** - Enter a name for the adapter (2-40 characters).
   * **Description** (optional) - Enter a description for the adapter (2-240 characters).
   * **Root key** - Select the root key to use for this adapter. The root key encrypts the KMIP keys that the adapter creates. Your root key must be in an `active` state for your adapter to function correctly.

4. Optional: Add a public TLS certificate to allow the holder of the corresponding private certificate to communicate with {{site.data.keyword.keymanagementserviceshort}} through the KMIP adapter. Only authorized certificates can make KMIP protocol requests against your instance.

   To add a certificate:
   1. Click **Add**.
   2. Enter a name for the certificate.
   3. Enter the certificate contents in PEM format, including the `BEGIN CERTIFICATE` and `END CERTIFICATE` tags.
   4. Click **Add certificate**.

   Certificate association can take a few minutes. A certificate can only be associated with a single adapter in a {{site.data.keyword.keymanagementserviceshort}} region.

Resources managed through the KMIP protocol cannot be accessed through the HTTP API.
{: note}

Keep the private key of any uploaded certificates secure. Any certificate uploaded to a KMIP adapter can make all supported KMIP operations.
{: important}

## Configuring a KMIP client to communicate with an adapter
{: #kmip-client}

To communicate with your adapter, you must either [set up VMware](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vsphere-security.html){: external} or create a KMIP client that can communicate over TCP with mTLS and send messages using the TTLV message format [as described in the KMIP specifications](https://docs.oasis-open.org/kmip/spec/v1.4/os/kmip-spec-v1.4-os.html#_Toc490660910){: external}.

For VMware vSphere, follow the steps in [Add a Standard Key Provider Using the vSphere Client](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vsphere-security.html){: external}. When you add a standard key provider, use the [{{site.data.keyword.keymanagementserviceshort}} endpoint](/docs/key-protect?topic=key-protect-regions) specific to your instance's region. For example, for a {{site.data.keyword.keymanagementserviceshort}} instance in the `us-south` region, use `us-south.kms.cloud.ibm.com` as the address and `5696` as the port.

The vSphere client must upload its client certificate to the adapter to communicate with the KMIP adapter. Follow the steps in [Use the Certificate Option to Establish a Standard Key Provider Trusted Connection](https://techdocs.broadcom.com/us/en/vmware-cis/vsphere/vsphere/7-0/vsphere-security.html#GUID-5797AA3E-98EC-4190-A2BB-8E5A3E5F9820){: external} to download the client certificate, then upload it to the adapter.


## Granting access to KMIP 
{: #kmip-granting-access}

Review [roles and permissions](/docs/key-protect?topic=key-protect-manage-access) to learn how {{site.data.keyword.cloud_notm}} IAM roles map to {{site.data.keyword.keymanagementserviceshort}} actions.
{: tip}

The following IAM actions govern resources that will be used to manage access to KMIP resources:

- `kms.kmip-management.create`
- `kms.kmip-management.list`
- `kms.kmip-management.read`
- `kms.kmip-management.delete`

Each action grants the mentioned behavior to all `kmip_adapter` `certificate` and `kmip_object` resources in the instance, without granularity.

## Viewing and updating adapter details
{: #kmip-adapter-view}
{: ui}

The adapter details panel displays information about an adapter and allows you to perform actions such as adding certificates.

To view adapter details:

1. Click the actions menu (⋯) for the adapter.
2. Select **Details**.

The details panel displays the adapter's name, description, associated KMIP symmetric keys, and uploaded certificates. You can also upload additional certificates from this panel.

KMIP symmetric keys cannot be deleted using the console. To delete keys, use the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference&interface=ui#kp-kmip-object-delete). Only KMIP symmetric keys that are not in the `Active` state (state `1`) can be deleted. You cannot delete an adapter if it contains keys in the `Active` state.

Each adapter's resources are protected with a root key. You cannot delete a root key that is active and associated with an adapter.

Each KMIP symmetric key that is created counts as a single key version and incurs a [charge of one key version](/docs/key-protect?topic=key-protect-pricing-plan). Deletion of a KMIP symmetric key is permanent.
{: important}

## KMIP supported objects and operations
{: #kmip-supported}

Refer to [Result Reason](http://docs.oasis-open.org/kmip/spec/v1.4/os/kmip-spec-v1.4-os.html#_Toc490660896){: external} in the KMIP Version 1.4 documentation for the reasons for expected failures, such as a request against an unsupported operation.
{: note}

### KMIP supported operations
{: #kmip-supported-operations}

Only the following operations are supported.
{: important}

| Section | Operation | Summary |
| ------- | --------- | ------- |
| 4.1 | Create | Creates a KMIP object. |
| 4.9 | Locate | Searches for objects matching the given criteria or attribute metadata. |
| 4.11 | Get | Retrieves object information, specifically the key material. |
| 4.12 | Get Attributes | Retrieves attribute metadata about the object. |
| 4.14 | Add Attribute | Adds attribute metadata to the object. |
| 4.19 | Activate | Sets the object to an "Active" state. The object cannot be destroyed while in the active state. |
| 4.20 | Revoke | Sets the object to a "Compromised" state if the revocation reason code is "Key Compromise" or "CA Compromise". Otherwise, sets the object to a "Deactivated" state. |
| 4.21 | Destroy | Destroys the key material of the object. This action cannot be reversed. |
| 4.26 | Discover Versions | Requests supported KMIP protocol versions from the server. Only v1.4 is returned. |
{: caption="Supported KMIP operations" caption-side="bottom"}

### Supported objects
{: #kmip-supported-objects}

| Section | Object |
| ------- | ------ |
| 2.2 | Symmetric Key |
{: caption="Supported KMIP objects" caption-side="bottom"}

## Creating and using KMIP adapters in the API
{: #kmip-adapter-api}
{: api}

This section describes how to use KMIP adapters of profile `native_1.0` with the API, including adding and removing KMIP client certificates and viewing and deleting KMIP objects.

You can create a KMIP adapter by making a `POST` call to the following endpoint. 

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters
```
{: codeblock}

Operations on KMIP adapter subresources, including KMIP client certificates and KMIP objects will be in the following endpoints:

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_name_or_ID>/certificates
https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_name_or_ID>/kmip_objects
```
{: codeblock}

1. [Retrieve authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Copy the ID of the root key that you want to use to create your KMIP adapter.

    You can find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

3. Create a KMIP adapter with the following `curl` command: 

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters" \
        -H "accept: application/vnd.ibm.kms.kmip_adapter+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.kmip_adapter+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.kmip_adapter+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                    "name": "<adapter_name>",
                    "description": "<adapter_description>",
                    "profile": "native_1.0",
                    "profile_data": {
                        "crk_id": "<root_keyID_or_alias>"
                    }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br> For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|root_keyID_or_alias|**Required**. The unique identifier or alias for the root key that you want to use for the adapter.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|adapter_name|**Optional**. A human-readable name of the KMIP adapter unique within the kms instance. If one is not specified, one will be autogenerated of the format `kmip_adapter_<random_string>`. To protect your privacy do not use personal data, such as your name or location, as a name for your KMIP adapter. The name must be alphanumeric and cannot contain spaces or special characters other than - or _. The name cannot be a UUID.|
|adapter_description|**Optional** The KMIP adapter's description. The maximum length is 240 characters. To protect your privacy, do not use personal data, such as your name or location, as a description for your KMIP adapter.|
{: caption="Describes the variables that are needed to create a KMIP adapter in {{site.data.keyword.keymanagementserviceshort}}." caption-side="bottom"}

4. Optional: you can list KMIP adapters that exist in an instance with the following `curl` command:

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters" \
        -H "accept: application/vnd.ibm.kms.kmip_adapter+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.kmip_adapter+json"
    ```
    {: codeblock}

    You can also get a specific KMIP adapter by using the following `curl` command:

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_name_or_ID>" \
        -H "accept: application/vnd.ibm.kms.kmip_adapter+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.kmip_adapter+json"
    ```
    {: codeblock}

    Note that you can use either the adapter's UUID or the adapter's name to get a specific adapter.

5. You can delete a KMIP adapter with the following `curl` command:

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_name_or_ID>" \
        -H "accept: application/vnd.ibm.kms.kmip_adapter+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.kmip_adapter+json"
    ```
    {: codeblock}

    You can only delete the KMIP adapter if all the KMIP objects under the adapter are deleted.

### Adding a KMIP client certificate to a KMIP adapter
{: #kmip-adapter-api-cert}
{: api}

After you create a KMIP adapter, you can add a KMIP client certificate to associate with the adapter. After a certificate is registered, you can use it to communicate with the KMIP server with mTLS as described in the KMIP specifications. Certificate registration can take up to five minutes. Certificates must be unique within the same region.

1. [Retrieve authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Identify the KMIP adapter you want to add your certificate to.

3. Add the KMIP client certificate with the following `curl` command:

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/certificates" \
        -H "accept: application/vnd.ibm.kms.kmip_client_certificate+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.kmip_client_certificate+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.kmip_client_certificate+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                    "certificate": "<certificate_pem>",
                    "name": "<certificate_name>"
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br> For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|adapter_id|**Required**. The unique identifier or name for KMIP adapter you want to register the certificate with.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|certificate_pem|**Required** The contents of the KMIP client certificate. It must be in the x509 PEM format. It should explicitly have the BEGIN CERTIFICATE and END CERTIFICATE tags.|
|certificate_name|**Optional**. A human-readable name that uniquely identifies a certificate within the given adapter. If one is not specified, one will be autogenerated of the format `kmip_cert_<random_string>`. To protect your privacy do not use personal data, such as your name or location, as a name for your KMIP adapter. The name must be alphanumeric and cannot contain spaces or special characters other than - or _. The name cannot be a UUID.|
{: caption="Describes the variables that are needed to create a KMIP client certificate in {{site.data.keyword.keymanagementserviceshort}}." caption-side="bottom"}

4. Optional: you can list KMIP client certificates associated with an adapter with the following `curl` command:

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/certificates" \
        -H "accept: application/vnd.ibm.kms.kmip_client_certificate+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    You can also get a specific KMIP client certificate by using the following `curl` command:

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/certificates/<certificate_name_or_id>" \
        -H "accept: application/vnd.ibm.kms.kmip_client_certificate+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" 
    ```
    {: codeblock}

    Note that you can use either the certificate's UUID or the certificate's name to get a specific adapter.

5. You can delete a KMIP client certificate with the following `curl` command:

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_name_or_ID>" \
        -H "accept: application/vnd.ibm.kms.kmip_adapter+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    You can only delete the KMIP adapter if all the KMIP objects under the adapter are deleted.

### Viewing and deleting KMIP objects within an adapter
{: #kmip-adapter-api-view-delete-objects}
{: api}

KMIP objects cannot be created through the REST API, but they can be viewed and deleted.

1. [Retrieve authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Identify the KMIP adapter you want to add your certificate to.

3. You can view KMIP objects within a KMIP adapter with the following `curl` command:

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/kmip_objects" \
        -H "accept: application/vnd.ibm.kms.kmip_object+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

4. You can view a specific KMIP object within a KMIP adapter with the following `curl` command:

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/kmip_objects/<object_id>" \
        -H "accept: application/vnd.ibm.kms.kmip_object+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

5. You can delete a specific KMIP object within a KMIP adapter with the following `curl` command:

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/kmip_adapters/<adapter_id>/kmip_objects/<object_id>" \
        -H "accept: application/vnd.ibm.kms.kmip_object+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Where the `<object_id>` is the UUID of the KMIP object. You cannot delete KMIP objects in the Active (state=2) state.
