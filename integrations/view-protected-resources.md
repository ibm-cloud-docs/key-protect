---

copyright:
  years: 2020, 2024
lastupdated: "2024-10-09"

keywords: view protected data, view encrypted resources, registrations

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

# Viewing associations between root keys and encrypted {{site.data.keyword.cloud_notm}} resources
{: #view-protected-resources}

You can view associations between root keys and other cloud resources, such as
Cloud Object Storage buckets or Cloud Databases deployments, by using the
{{site.data.keyword.keymanagementservicelong_notm}} API.
{: shortdesc}

When you use a root key to protect at rest data with envelope encryption, the
cloud services that use the key can create a registration between the key and
the resource that it protects.

Registrations are associations between keys and cloud resources that help you
get a full view of which encryption keys protect what data on
{{site.data.keyword.cloud_notm}}.

|Benefit|Description|
|--- |--- |
|Centralized view of protected resources|As an administrator for your {{site.data.keyword.keymanagementserviceshort}} instance, you want to quickly understand which cloud resources are protected by a root key.|
|Security and compliance|As a security admin, you need a way to determine the risk that's involved with [destroying a root key](/docs/key-protect?topic=key-protect-delete-keys).<br>You want to examine which keys are actively protecting what data so that you can evaluate exposures based on your organization's security or compliance needs.|
{: caption="Describes the benefits of key registration." caption-side="top"}

Key registration is an extra feature that's available only if the cloud service
has enabled it as part of its integration with
{{site.data.keyword.keymanagementserviceshort}}. To determine whether an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
supports key registration, refer to its service documentation for more
information.
{: note}

## Viewing protected resources in the console
{: #view-protected-resources-console}

You can browse the registrations that are available between your
{{site.data.keyword.keymanagementserviceshort}} keys and cloud resources by
using the {{site.data.keyword.keymanagementserviceshort}} IBM Cloud console.

### Viewing protected resources in your instance
{: #view-protected-resources-console-instance}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Select the `Associated resources` link on the left side menu.

5. On the Associated resources page, use the **Associated Resources** table to
    browse the registrations in your service.

6. Click the `^` icon under the `Details` column to view a list of details for a
    specific registration.

7. Click `Filter` button to filter for resources by key ID, Cloud Resource Name
    (CRN), and retention policy.

### Viewing protected resources associated with your key
{: #view-protected-resources-console-key}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in
    your service.

5. Click the ⋯ icon to open a list of options for the key.

6. From the options menu, click **Key associated resources** to view the key's
    associated registrations.

## Viewing protected resources with the API
{: #view-protected-resources-api}

You can browse the registrations that are available between your
{{site.data.keyword.keymanagementserviceshort}} keys and cloud resources by
using the {{site.data.keyword.keymanagementserviceshort}} API.

For example, when you call `GET api/v2/keys/{id}/registrations`,
{{site.data.keyword.keymanagementserviceshort}} returns details about the key
registration. The following JSON output represents a registration between a key
and a cloud resource.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.registration+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "keyId": "02fd6835-6001-4482-a892-13bd2085f75d",
            "resourceCrn": "crn:v1:bluemix:public:<service-name>:<region>:a/<account-id>:<service-instance>:bucket:<bucket-name>",
            "createdBy": "IBMid-25555555",
            "creationDate": "2010-01-12T05:23:19+0000",
            "updatedBy": "IBMid-25555555",
            "lastUpdated": "2010-01-12T05:23:19+0000",
            "description": "A description of the registration",
            "preventKeyDeletion": true,
            "keyVersion": {
                "id": "02fd6835-6001-4482-a892-13bd2085f75d",
                "creationDate": "2010-01-12T05:23:19+0000"
            }
        }
    ]
}
```
{: screen}

The following table describes the properties of a registration.

|Parameter|Description|
|--- |--- |
|keyID|The ID that identifies the root key that is associated with the cloud resource.|
|resourceCrn|The Cloud Resource Name (CRN) that represents the cloud resource, such as a Cloud Object Storage bucket, that is associated with the key.|
|createdBy|The unique identifier for the resource that created the registration.|
|creationDate|The date the registration was created.|
|lastUpdated|The date the registration was updated.|
|description|A description for the registration.|
|preventKeyDeletion|A boolean that determines whether {{site.data.keyword.keymanagementserviceshort}} must prevent deletion of the root key. If true, the associated resource is non-erasable due to a retention policy, and the {{site.data.keyword.keymanagementserviceshort}} key that is encrypting the resource cannot be deleted.|
|keyVersion|The version of the root key that's protecting the cloud resource.|
{: caption="Properties that are associated with a registration." caption-side="top"}

### Listing registrations for a specific root key
{: #view-protected-resources-key-id}

You can retrieve the registration details that are associated with a specific
root key by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/registrations
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. View the registrations that are associated with a root key by running the
    following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/registrations" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The identifier or alias for the root key that is associated with the cloud resources that you want to view.<br><br>For more information, see [View Keys](/docs/key-protect?topic=key-protect-view-keys).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Describes the variables that are needed to list all registrations that are associated with a root key." caption-side="top"}

A successful `GET api/v2/keys/<keyID_or_alias>/registrations` request returns a
collection of registrations that are mapped to the specified key ID.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.registration+json",
        "collectionTotal": 2
    },
    "resources": [
        {
            "keyId": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6",
            "resourceCrn": "crn:v1:bluemix:public:cloud-object-storage:global:a/<account-id>:<service-instance>:bucket:<bucket-name>",
            "createdBy": "IBMid-25555555",
            "creationDate": "2010-01-12T05:23:19+0000",
            "updatedBy": "IBMid-25555555",
            "lastUpdated": "2010-01-12T05:23:19+0000",
            "description": "A description of the registration",
            "preventKeyDeletion": true,
            "keyVersion": {
                "id": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6",
                "creationDate": "2010-01-12T05:23:19+0000"
            }
        },
        {
            "keyId": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "resourceCrn": "crn:v1:bluemix:public:cloud-object-storage:global:a/<account-id>:<service-instance>:bucket:<other-bucket-name>",
            "createdBy": "IBMid-25555555",
            "creationDate": "2010-01-12T05:23:19+0000",
            "updatedBy": "IBMid-25555555",
            "lastUpdated": "2010-01-12T05:23:19+0000",
            "description": "A description of the registration",
            "preventKeyDeletion": true,
            "keyVersion": {
                "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
                "creationDate": "2010-01-12T05:23:19+0000"
            }
        }
    ]
}
```
{: screen}

The `resourceCrn` value represents the unique identifier of the cloud
resource that is encrypted by `keyId`. The metadata that is associated with
the registration, such as its creation date, is also returned in the
response body.

By default, `GET api/v2/keys/registrations` returns the first 200
registrations, but you can adjust this limit by using the `limit` parameter
at query time.
{: note}

#### Filter registrations for a specific root key
{: #filter-registrations-specifc-key-api}

You can filter for a set of registrations that are associated with a root key
by specifying the `preventKeyDeletion` and `urlEncodedResourceCRNQuery`
parameters at query time.

For example, you might have 25 total registrations that are stored in your
{{site.data.keyword.keymanagementserviceshort}} instance, but you only
want to retrieve registrations that have a retention policy that is associated
with a specific Cloud Resource Name (CRN).

You can use the following example request to retrieve a filtered set of
registrations.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/registrations?preventKeyDeletion=<true|false>&urlEncodedResourceCRNQuery=<url_encoded_CRN>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `preventKeyDeletion` and `urlEncodedResourceCRNQuery` variables in
your request according to the following table.

|Variable|Description|
|--- |--- |
|preventKeyDeletion|A boolean that filters registrations based on if a registered resource  has a retention policy.<br><br>For example, if you have multiple registrations in your instance, and you want to list only registrations where preventKeyDeletion is `true`, use `../registrations?preventKeyDeletion=true`.<br><br>You can also pair preventKeyDeletion with offest, limit, and urlEncodedResourceCRNQuery to search through your available resources.|
|urlEncodedResourceCRNQuery|The resource CRN that you want to filter registrations by.<br><br>For example, if you have multiple registrations in your instance, and you want to only view registrations that are associated with a specific Cloud Resource Name (CRN), use `../registrations?urlEncodedResourceCRNQuery="url_encoded_CRN"`.<br><br> For more information, see [CRN query examples](#crn-query-examples).<br><br>You can also pair urlEncodedResourceCRNQuery with offest, limit, and preventKeyDeletion to search through your available resources.|
{: caption="Describes the preventKeyDeletion and  urlEncodedResourceCRNQuery variables." caption-side="top"}

You can also filter for a subset of registrations by specifying the `limit` and
`offset` parameters at query time.

You can use the following example request to retrieve a filtered set of
registrations.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/registrations?offset=<offset>&limit=<limit>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `limit` and `offset` variables in your request according to the
following table.

|Variable|Description|
|--- |--- |
|offset|The number of registrations to skip.<br><br>For example, if you have 50 registrations in your instance, and you want to list registrations 26 - 50, use `../registrations?offset=25`.<br><br>You can also pair offset with limit to page through your available resources.|
|limit|The number of registrations to retrieve.<br><br>For example, if you have 100 registrations in your instance, and you want to list only 10 registrations, use `../registrations?limit=10`. The maximum value for limit is 5000.<br><br>You can also pair offset with limit to page through your available resources.|
{: caption="Describes the limit and offset variables." caption-side="top"}

### Listing registrations for any root key
{: #view-protected-resources-any-key}

You can also retrieve a list of registrations that are associated with any cloud
resource by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. View the registrations that match a CRN query that you specify by running the
    following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Describes the variables that are needed to list registrations for any key in your {{site.data.keyword.keymanagementserviceshort}} instance." caption-side="top"}

#### Filter registrations for any root key
{: #filter-registrations-any-key-api}

You can filter for a set of registrations that are associated with any root key
managed in your provisioned instance of
{{site.data.keyword.keymanagementserviceshort}}
by specifying the `preventKeyDeletion` and `urlEncodedResourceCRNQuery`
parameters at query time.

For example, you might have 25 total registrations that are stored in your
{{site.data.keyword.keymanagementserviceshort}} instance, but you only
want to retrieve registrations that have a retention policy this is associated
with a specific Cloud Resource Name (CRN).

You can use the following example request to retrieve a specific set of
registrations.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations?preventKeyDeletion=<true|false>&urlEncodedResourceCRNQuery=<url_encoded_CRN>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `preventKeyDeletion` and `urlEncodedResourceCRNQuery` variables in
your request according to the following table.

|Variable|Description|
|--- |--- |
|preventKeyDeletion|A boolean that filters registrations based on if a registered resource has a retention policy.<br><br>For example, if you have multiple registrations in your instance, and you want to list only registrations where preventKeyDeletion is `true`, use `../registrations?preventKeyDeletion=true`.<br><br>You can also pair preventKeyDeletion with offest, limit, and urlEncodedResourceCRNQuery to search through your available resources.|
|urlEncodedResourceCRNQuery|The resource CRN that you want to filter registrations by.<br><br>For example, if you have multiple registrations in your instance, and you want to only view registrations that are associated with a specific Cloud Resource Name (CRN), use `../registrations?urlEncodedResourceCRNQuery="url_encoded_CRN"`.<br><br>For more information, see [CRN query examples](#crn-query-examples).<br><br>You can also pair urlEncodedResourceCRNQuery with offest, limit, and preventKeyDeletion to search through your available resources.|
{: caption="Describes the preventKeyDeletion and urlEncodedResourceCRNQuery variables." caption-side="top"}

You can also filter for a subset of registrations by specifying the `limit` and
`offset` parameters at query time.

You can use the following example request to retrieve a different set of
registrations.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations?offset=<offset>&limit=<limit>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `limit` and `offset` variables in your request according to the
following table.

|Variable|Description|
|--- |--- |
|offset|The number of registrations to skip.<br><br>For example, if you have 50 registrations in your instance, and you want to list registrations 26 - 50, use `../registrations?offset=25`.<br><br>You can also pair offset with limit to page through your available resources.|
|limit|The number of registrations to retrieve.<br><br>For example, if you have 100 registrations in your instance, and you want to list only 10 registrations, use `../registrations?limit=10`. The maximum value for limit is 5000.<br><br>You can also pair offset with limit to page through your available resources.|
{: caption="Describes the limit and offset variables." caption-side="top"}

### CRN query examples
{: #crn-query-examples}

Use URL encoded CRN queries to filter registrations by
{{site.data.keyword.keymanagementserviceshort}} instance, resource type, or
resource name. To learn more about CRN segments and format, see
[Cloud Resource Names](/docs/account?topic=account-crn){: external}.

Cloud Services that use {{site.data.keyword.keymanagementserviceshort}}
to associate keys with resources on your behalf can only view or query for CRNs
that match the first 8 segments of their service CRN.
{: note}

- To search for the existence of a registration up to a specific CRN segment,
    use a colon followed by an asterisk (`*`).

    ```plaintext
    crn:v1:bluemix:public:databases-for-redis:us-south:a/
    274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7:*:*
    ```
    {: screen}

    This query returns Databases for Redis registrations that are associated
    with all resource types and names for deployment ID
    _29caf0e7-120f-4da8-9551-3abf57ebcfc7_.

- To search for existence of a registration up to a specific CRN segment that's
    prefixed by `<string>`, use a colon followed by `<string>*` on the last
    segment of the CRN query.

    ```plaintext
    crn:v1:bluemix:public:cloud-object-storage:global:a/e1bb63d6a20dc57c87501ac4c4c99dcb:*:bucket:prod*
    ```
    {: screen}

    This query returns all Cloud Object Storage bucket registrations within
    account _e1bb63d6a20dc57c87501ac4c4c99dcb_ that are prefixed by `prod`.

When
[listing registrations that are associated with any root key](/apidocs/key-protect#getregistrationsallkeys){: external}, your CRN query should not contain an asterisk (*) in the first eight segments.
{: note}

The following tables provides a list of CRN query examples before and after URL
encoding. To view the URL encoded values, click the **URL encoded** tab.

| Value |
| ----- |
|`crn:v1:bluemix:public:databases-for-redis:us-south:a/274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7:*:*` |
|`crn:v1:bluemix:public:cloud-object-storage:global:a/e1bb63d6a20dc57c87501ac4c4c99dcb:*:bucket:prod*` |
|`crn:v1:bluemix:public:cloudantnosqldb:us-south:a/f586c28d154d4c65a4a4a34cf75f55d0:94255ea3-af1c-41b7-9805-61f775e20702:*:prod*`. |
{: caption="CRN query examples." caption-side="top"}
{: #table-5}
{: tab-title="Original"}
{: class="simple-tab-table"}

| Value |
| ----- |
|`crn%3Av1%3Abluemix%3Apublic%3Adatabases-for-redis%3Aus-south%3Aa%2F274074dce64e9c423ffc238516c755e1%3A29caf0e7-120f-4da8-9551-3abf57ebcfc7%3A*%3A*` |
| `crn%3Av1%3Abluemix%3Apublic%3Acloud-object-storage%3Aglobal%3Aa%2Fe1bb63d6a20dc57c87501ac4c4c99dcb%3A*%3Abucket%3Aprod*` |
|`crn%3Av1%3Abluemix%3Apublic%3Acloudantnosqldb%3Aus-south%3Aa%2Ff586c28d154d4c65a4a4a34cf75f55d0%3A94255ea3-af1c-41b7-9805-61f775e20702%3A%2A%3Aprod%2A` |
{: caption="CRN query examples." caption-side="top"}
{: #table-6}
{: tab-title="URL encoded"}
{: class="simple-tab-table"}

## What's next
{: #view-protected-resources-next-steps}

To find out more about viewing registrations,
[check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.
