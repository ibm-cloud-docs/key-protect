---

copyright:
  years: 2024
lastupdated: "2024-04-15"

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

To better facilitate the use of {{site.data.keyword.keymanagementservicefull}} keys to create key management interoperability protocol (KMIP) adapters for use with VMWare, {{site.data.keyword.keymanagementserviceshort}} now directly offers the ability to create adapters and upload certificates using the {{site.data.keyword.keymanagementserviceshort}} control plane (UI). 

This solution architecture describes the {{site.data.keyword.keymanagementserviceshort}} Native KMIP support architecture for protecting your VMware® instances. Many storage encryption options are available to protect your VMware workload. {{site.data.keyword.keymanagementserviceshort}} Native KMIP support works together with VMware native vSphere encryption and vSAN™ encryption. The vSphere and vSAN encryption provides simplified storage encryption management together with the security and flexibility of {{site.data.keyword.cloud}} {{site.data.keyword.keymanagementserviceshort}} customer-managed keys.

This solution is an alternative to the [KMIP for VMWare](/docs/vmwaresolutions?topic=vmwaresolutions-kmip-overview) solution offering on {{site.data.keyword.cloud_notm}}. As a result, this document doesn't cover the existing configuration of these foundation solutions on {{site.data.keyword.cloud_notm}}. To understand more about the foundation solution architecture, see [Overview of VMware Solutions](/docs/vmwaresolutions?topic=vmwaresolutions-solution_overview).

This feature works in parallel with the current KMIP for VMWare solution. It is not currently possible to import adapters created using the VMWare solution into {{site.data.keyword.keymanagementserviceshort}} (or vice versa).
{: tip}

## Benefits
{: #kmip-overview-benefits}

While many storage encryption solutions are available for your VMware workload, {{site.data.keyword.keymanagementserviceshort}} Native KMIP support offers the following benefits:

* Integration with VMware vSAN encryption and vSphere encryption, both of which are implemented in the hypervisor layer rather than the storage or virtual machine layer. This approach allows easy management and transparency to your storage solution and application.
* Fully managed key management server available in many {{site.data.keyword.cloud_notm}} multizone regions (MZRs).
* Integrating your VMware cluster with {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}} provides you with fully customer-managed keys that you can revoke at any time.

## Creating an adapter
{: #kmip-adapter-create}
{: ui}

A maximum of 200 adapters can be created on a single instance. Each adapter can have a maximum of 200 certificates associated with it.
{: important}

KMIP adapters are created using {{site.data.keyword.keymanagementserviceshort}} root keys. If you do not have a root key, [create one](/docs/key-protect?topic=key-protect-create-root-keys).

To create an adapter: 

1. Navigate to the **KMIP Adapters** panel using the left navigation. If this is your first adapter, the table should be empty. Note that you must either have the [`Manager` role or the `KmipAdapterManager` role](/docs/key-protect?topic=key-protect-manage-access) on the instance in order to create an adapter.

2. Find the **Create Adapter** button and click it.

3. In the open side panel, give the adapter a **Name** and, optionally, a **Description**. Note that names must contain at least two and no more than 40 characters. If you choose to give it a description, it must be at least two and no more than 240 characters. After that has been completed, you can choose the root key you want to use to create this adapter and to encrypt the KMIP keys it creates. Choosing a root key is mandatory. Note that your root key must be in an `active` state for your adapter to function correctly.

4. Adding a public TLS certificate allows the holder of its corresponding private certificate to communicate with {{site.data.keyword.keymanagementserviceshort}} via its associated KMIP adapter. Only certificates authorized under a KMIP adapter will be permitted to make KMIP protocol requests against your instance. Note that resources managed via the KMIP protocol cannot be accessed via HTTP API.

   While you do not need to add any certificates during the creation of the adapter, if you know which certificates you want to add, you can do so by clicking the **Add certificates (optional)** tab in the panel. In the resulting screen, click the **Upload certificate** button, give the certificate a name, and input the contents of the certificate (the material must be in PEM format and contain the `BEGIN CERTIFICATE` and `END CERTIFICATE` tags). Note that it can take a few minutes for the certificate to be associated. Also, a certificate can only be associated with a single adapter in a {{site.data.keyword.keymanagementserviceshort}} region.

Keep the private key of any uploaded certificates secure, as any certificate uploaded to a KMIP adapter will have the ability to make all supported KMIP operations.
{: important}

## Configuring a KMIP client to communicate with an adapter
{: #kmip-client}

To communicate with your adapter, you must either [setup VMWare](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-55B17A13-13AB-4CA1-8D83-6515AA1FEC67.html){: external} (which will take care of your communications to your client, once you have associated the certificate provided by VMWare to it), or have created a KMIP client that can communicate over TCP with mTLS and can send messages using the TTLV message format [as described in KMIP specifications](https://docs.oasis-open.org/kmip/spec/v1.4/os/kmip-spec-v1.4-os.html#_Toc490660910){: external}.

For VMWare vSphere, follow the steps outlined in [Add a Standard Key Provider Using the vSphere Client](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-55B17A13-13AB-4CA1-8D83-6515AA1FEC67.html). When adding a standard key provider, use the [{{site.data.keyword.keymanagementserviceshort}} endpoint](https://cloud.ibm.com/docs/key-protect?topic=key-protect-regions) specific to your instance's region. For example, for the {{site.data.keyword.keymanagementserviceshort}} instance in region `us-south` , use `us-south.kms.cloud.ibm.com` as the address and `5696` as the port, which is the default for the KMIP server. 

As described in step 4 above, the vSphere client needs to upload its client certificate in the adapter in order for it to communicate with the KMIP adapter. Follow the steps outlined in [Use the Certificate Option to Establish a Standard Key Provider Trusted Connection](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-5797AA3E-98EC-4190-A2BB-8E5A3E5F9820.html#GUID-5797AA3E-98EC-4190-A2BB-8E5A3E5F9820) guide to download the client certificate. This certificate can be uploaded into the adapter.


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

The adapter details panel allows you to learn about an adapter (for example, through its description) and also allows you to do actions like adding certificates.

To view the adapter details, click the ⋯ icon. This shows all of the details about the adapter. Here you can see its name, its description, any KMIP symmetric keys associated with the adapter, and certificates that have been uploaded to it. You can also use this panel to upload more certificates, as desired.

KMIP symmetric keys cannot be deleted using the UI. To delete keys, you must use the [CLI](/docs/key-protect?topic=key-protect-key-protect-cli-reference&interface=ui#kp-kmip-object-delete). Only KMIP symmetric keys that are in a state other than the `Active` (state `1`) can be deleted. An adapter cannot be deleted if it contains keys that are in the `Active` state.

Each adapter's resources are protected with a CRK. You cannot delete a CRK that is active and associated with an adapter.

Each KMIP symmetric key that is created counts as a single key version and may incur a [charge, depending on the number of key versions you have](/docs/key-protect?topic=key-protect-pricing-plan). If you want to delete a KMIP symmetric key, you can only do so using the API, CLI, or SDK. You cannot use the UI. Note that deletions of a KMIP symmetric is permanent.
{:important}

## KMIP supported objects and operations
{: #kmip-supported}

Refer to [Result Reason](http://docs.oasis-open.org/kmip/spec/v1.4/os/kmip-spec-v1.4-os.html#_Toc490660896){: external} in the KMIP Version 1.4 documentation for the reasons for expected failures, such as a request against an unsupported operation.
{: note}

### KMIP supported operations
{: #kmip-supported-operations}

Only these operations are supported.
{:important}

|  | Operation | Summary |
| - | - | - |
| 4.1 | Create | Creates a KMIP object
| 4.11 | Get | Retrieve object information, specifically the key material.
| 4.12 | Get Attributes | Retrieve attribute metadata about the object.
| 4.14 | Add Attribute | Add attribute metadata to the object.
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

## Creating and using KMIP adapters in the API
{: #kmip-adapter-api}
{: api}

This will walk through using KMIP adapters of profile `native_1.0` using the API, adding and removing KMIP client certificates from the adapter, then viewing and deleting KMIP objects associated with the adapter.

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
{: caption="Table 1. Describes the variables that are needed to create a KMIP adapter in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

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

Now that a KMIP adapter has been created, you can add a KMIP client certificate to be associated with the KMIP adapter. Once a certificate has been registered, you can use that certificate to communicate with the KMIP server with mTLS as described by the KMIP specifications. It may take up to five minutes from the certificate being registered to the certificate being usable with the KMIP server. Certificates must be unique within the same region. 

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
{: caption="Table 2. Describes the variables that are needed to create a KMIP client certificate in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

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