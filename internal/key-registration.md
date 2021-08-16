---

copyright:
  years: 2019, 2020
lastupdated: "2020-08-22"

keywords: enable BYOK, onboard to Key Protect, Key Protect onboarding, internal, key registration, KYOK, BYOK

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}

# Register protected resources
{: #register-protected-resources}

Before you protect a data encryption key (DEK) with a root key, register the
cloud resource with {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

This content is currently being developed and reviewed. For questions about key
registration, reach out to us in the `#keyprotect-byok-adopters-channel` Slack
channel.
{: note}

When you register cloud resources with
{{site.data.keyword.keymanagementserviceshort}}, IBM Cloud customers can:

- View how root keys map to resources across the cloud

- Understand which cloud resources are protected by root keys

- Determine the risk involved with destroying a root key

- Evaluate the blast radius if a key becomes compromised

## Planning for registration
{: #plan-for-registration}

<dl>
    <dt>
        Registering new resources
    </dt>
    <dd>
        When your customers create a new resource, such as a Cloud Object Storage
    bucket, they can choose to encrypt the resource with an existing BYOK key.
    At resource creation, your service must
    [create a registration](#create-registration)
    between the new
    resource and the {{site.data.keyword.keymanagementserviceshort}} key.
    </dd>

    <dt>
        Registering existing resources
    </dt>
    <dd>
        If your service is already integrated with
    {{site.data.keyword.keymanagementserviceshort}} for Bring Your Own Key
    (BYOK), your service must
    [register existing resources](#register-existing-resources)
    retroactively. When your service registers all existing resources, customers
    can see a full view of the cloud resources that are already protected by
    {{site.data.keyword.keymanagementserviceshort}} keys.
    </dd>
</dl>

## Registering resources
{: #register-resources}

As an integrated service, you can call the registration APIs to create, replace,
update, list, and delete registrations between a resource and a root key.

The following table shows the API methods that are available for creating and
modifying registrations.

| API methods | Description |
| ----------- | ----------- |
| `POST api/v2/keys/{id}/registrations/{url_encoded_crn}` | [Create a registration](/apidocs/key-protect#create-a-registration){: external} |
| `PUT api/v2/keys/{id}/registrations/{url_encoded_crn}` | [Replace a registration](/apidocs/key-protect#replace-a-registration){: external} |
| `PATCH api/v2/keys/{id}/registrations/{url_encoded_crn}` | [Update a registration](/apidocs/key-protect#update-a-registration){: external} |
| `DELETE api/v2/keys/{id}/registrations/{url_encoded_crn}` | [Delete a registration](/apidocs/key-protect#delete-a-registration){: external} |

### Step 1. Create a registration
{: #create-registration}

To associate a resource to a root key, make a `POST` call to the following
endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations/<url_encoded_CRN>
```

#### Example request
{: #example-registration-create}

```sh
$ curl -X POST \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations/<url_encoded_CRN>" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "content-type: application/json" \
    -d '{
            "metadata": {
                "collectionType": "application/vnd.ibm.kms.registration_input+json",
                "collectionTotal": 1
            },
            "resources": [
                {
                    "description": "<external_description>",
                    "registrationMetadata": <internal_metadata>,
                    "preventKeyDeletion": <true|false>
                }
            ]
        }'
```
{: codeblock}

Replace the variables in the example request according to the following table.

| Variable             | Description |
| -------------------- | ----------- |
| region               | **Required.** The region abbreviation, such as `us-east`, that represents the geographic area where the {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| key_ID               | **Required.** The unique identifier for the customer's root key that protects the cloud resource. |
| url_encoded_CRN      | **Required.** The URL encoded [Cloud Resource Name (CRN)](/docs/account?topic=account-crn){: external} that uniquely identifies the cloud resource. At minimum, provide a CRN that includes up to the `service-instance` segment. |
| CRN_token            | **Required.** Your [service's CRN token](/docs/get-coding?topic=get-coding-servicetoservice#create_crn_token){: external}. Include the full contents of the token, including the Bearer value, in the `curl` request. |
| instance_ID          | **Required.** The unique identifier that is assigned to customer's {{site.data.keyword.keymanagementserviceshort}} instance. |
| description          | A meaningful description in the context of your cloud service that describes the resource that is being protected by the root key. This field is exposed to customers when they use {{site.data.keyword.keymanagementserviceshort}} to review registered resources. |
| registrationMetadata | A text field that cloud services can use to store internal metadata about the registration. This field is not exposed to customers and is visible only by using IBM Cloud service to service calls. |
| preventKeyDeletion   | A boolean that determines whether {{site.data.keyword.keymanagementserviceshort}} must prevent deletion of a root key due to a Write Once Read Many (WORM) policy set on the customer resource.<br><br>If set to `true`, {{site.data.keyword.keymanagementserviceshort}} prevents deletion of the root key and its associated protected resources. The system prevents the deletion of any key that has at least one registration where `preventKeyDeletion` is `true`. |
{: caption="Table 1. Lists variables for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs" caption-side="top"}

### Step 2. Update the registration
{: #update-registration}

To modify a registration, make a `PATCH` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations/<url_encoded_CRN>
```

#### Example request
{: #example-update}

```sh
$ curl -X PATCH \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations/<url_encoded_CRN>" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "content-type: application/json" \
    -d '{
            "metadata": {
                "collectionType": "application/vnd.ibm.kms.registration_input+json",
                "collectionTotal": 1
            },
            "resources": [
                {
                    "description": "<external_description>",
                    "registrationMetadata": <internal_metadata>,
                    "preventKeyDeletion": <true|false>
                }
            ]
        }'
```
{: codeblock}

Replace the variables in the example request according to the following table.

| Variable             | Description |
| -------------------- | ----------- |
| region               | **Required.** The region abbreviation, such as `us-east`, that represents the geographic area where the {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| key_ID               | **Required.** The unique identifier for the customer's root key that protects the cloud resource. |
| url_encoded_CRN      | **Required.** The URL encoded [Cloud Resource Name (CRN)](/docs/account?topic=account-crn){: external} that uniquely identifies the cloud resource. At minimum, provide a CRN that includes up to the `service-instance` segment. |
| IAM_token            | **Required.** Your [service's CRN token](/docs/get-coding?topic=get-coding-servicetoservice#create_crn_token){: external}. Include the full contents of the token, including the Bearer value, in the `curl` request. |
| instance_ID          | **Required.** The unique identifier that is assigned to customer's {{site.data.keyword.keymanagementserviceshort}} instance. |
| description          | A meaningful description in the context of your cloud service that describes the resource that is being protected by the root key. This field is exposed to customers when they use {{site.data.keyword.keymanagementserviceshort}} to review registered resources. |
| registrationMetadata | A text field that cloud services can use to store internal metadata about the registration. This field is not exposed to customers and is visible only by using IBM Cloud service to service calls. |
| preventKeyDeletion   | A boolean that determines whether {{site.data.keyword.keymanagementserviceshort}} must prevent deletion of a root key due to a Write Once Read Many (WORM) policy set on the customer resource.<br><br>If set to `true`, {{site.data.keyword.keymanagementserviceshort}} prevents deletion of the root key and its associated protected resources. The system prevents the deletion of any key that has at least one registration where `preventKeyDeletion` is `true`. |
{: caption="Table 2. Lists variables for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs" caption-side="top"}

### Step 3. Delete the registration
{: #delete-registration}

After a resource is no longer within the cloud or protected by a root key,
de-register the association with the key.

#### Example request
{: #example-delete}

```sh
$ curl -X DELETE \
    "https://<endpoint>/api/v2/keys/<key_ID>/registrations/<url_encoded_CRN>" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the variables in the example request according to the following table.

| Variable        | Description |
| --------------- | ----------- |
| region          | **Required.** The region abbreviation, such as `us-east`, that represents the geographic area where the {{site.data.keyword.keymanagementserviceshort}}  instance resides. For more information, see [Service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| key_ID          | **Required.** The unique identifier for the customer's root key that protects the cloud resource. |
| url_encoded_CRN | **Required.** The URL encoded [Cloud Resource Name (CRN)](/docs/account?topic=account-crn){: external} that uniquely identifies the cloud resource. At minimum, provide a CRN that includes the `service-instance` segment. |
{: caption="Table 3. Lists variables for interacting with {{site.data.keyword.keymanagementserviceshort}} APIs" caption-side="top"}

## Registering existing resources
{: #register-existing-resources}

Bring Your Own Key (BYOK) was delivered without the key registration capability
initially, so your service might have resources that are already protected by a
root key. To create registrations for these resources, plan to register already
existing resources,

The desire is that all resources protect by a root key are registered so that it
enables all KYOK capabilities.

### Step 1. Identify your protected resources
{: #identify-protected-resources}

Your service likely has a place that stores root key IDs in some metadata that
associates your resource to the data. Identify all the affected resources, and
gather the following information:

- The Cloud Resource Name (CRN) of the resource that is protected (for example,
    a Cloud Object Storage bucket, or a Kubernetes Service cluster).

- The description of the protected resource using a string provided by the
    customer that is meaningful in the context of your service. Maybe that's the
    description, the name, or an ID, but provide that string so that when your
    customer sees it, they can quickly understand what it means.

- Information about the root key, which can be found in the root key's CRN
    - The {{site.data.keyword.keymanagementserviceshort}} instance GUID that
        contains the root key

    - The root key ID

### Step 2. Create a registration for each protected resource
{: #create-registration-existing-resource}

After your service has identified each protected resource, call the
{{site.data.keyword.keymanagementserviceshort}} APIs to
[create a registration](#create-registration)
for each resource.

## Retrieving Registered Resources
{: #view-registered-resources}

After you register a cloud service with
{{site.data.keyword.keymanagementserviceshort}}, use the
{{site.data.keyword.keymanagementserviceshort}} registration APIs to view the
association between your cloud resource and a root key.

The following table shows the API methods that are available for creating and
modifying registrations.

| API Method                            | Description |
| ------------------------------------- | ----------- |
| `GET /api/v2/keys/{id}/registrations` | [List registrations for a key](/apidocs/key-protect#list-registrations-for-a-key){: external} |
| `GET /api/v2/keys/registrations`      | [List registrations for any key](/apidocs/key-protect#list-registrations-for-any-key){: external} |
{: caption="Table 4. Lists {{site.data.keyword.keymanagementserviceshort}} API methods" caption-side="top"}

### Viewing CRNs associated with a registration
{: #view-protected-resource-crn}

In most cases your cloud resource can be associated with one registration, but
depending on the IBM Cloud service, your cloud resource can be associated with
many registrations. For example, IBM Cloud Database service might have a unique
identifier in the last segment of a CRN. The following table shows the CRN
format that uniquely identifies registrations from different cloud services:  

| Service              | CRN Format                      | Example |
| -------------------- | ------------------------------- | ------- |
| Cloud Object Storage | `crn:version:cname:ctype:service-name:location:scope:service-instance:bucket:bucketname`        | `crn:v1:bluemix:public:cloud-object-storage:global:a/59bcbfa6ea2f006b4ed7094c1a08dcff:76b98bfd-f730-47b8-b163-515187bbbbbb:bucket:mybucket` |
| ICD and Cloudant     | `crn:version:cname:ctype:service-name:location:scope:service-instance::/unique_identifier=xxxx` | Entry 1:<br>`crn:v1:bluemix:public:databases-for-postgresql:us-south:a/e1bb63d6a20dc57c87501ac4c4c99dcb:76b98bfd-f730-47b8-b163-515187bbbbbb::/agentid=1234`<br><br>Entry 2:<br>`crn:v1:bluemix:public:databases-for-postgresql:us-south:a/e1bb63d6a20dc57c87501ac4c4c99dcb:76b98bfd-f730-47b8-b163-515187bbbbbb::/agentid=4321` |
{: caption="Table 5. Lists examples of resource CRNs" caption-side="top"}

## Key states and service actions on a registration
{: #key-states-service-actions-registrations}

Key states affect whether an action that is performed on a registration succeeds
or fails. For example, if a key is in the Destroyed state, registrations cannot
be created with the key because it was previously deleted.

The following table describes how key states affect service registration
actions. The column headers represent the key states and the row headers
represent the actions that you can perform on a registration. The check mark
icon
(![Check mark icon](../../icons/checkmark-icon.svg)
indicates that the action on a registration is expected to succeed based on the
key state.

|Action|Active|Suspended|Deactivated|Destroyed|
|--- |--- |--- |--- |--- |
|Create Registration|![Check mark icon](../../icons/checkmark-icon.svg)||||
|Replace Registration|![Check mark icon](../../icons/checkmark-icon.svg)||||
|Update Registration|![Check mark icon](../../icons/checkmark-icon.svg)||||
|Delete Registration|![Check mark icon](../../icons/checkmark-icon.svg)|![Check mark icon](../../icons/checkmark-icon.svg)|||
|Get Registration|![Check mark icon](../../icons/checkmark-icon.svg)|![Check mark icon](../../icons/checkmark-icon.svg)|||
|List Registration|![Check mark icon](../../icons/checkmark-icon.svg)|![Check mark icon](../../icons/checkmark-icon.svg)||| 
{: caption="Table 6. Describes how key states affect service actions on registrations." caption-side="top"}

## Next steps
{: #registration-next-steps}

You've added a foundational capability that we can now build upon!

- Head over to
    [DEK rewrapping](/docs/key-protect?topic=key-protect-dek-rewrap)


