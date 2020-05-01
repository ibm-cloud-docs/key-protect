---

copyright:
  years: 2020
lastupdated: "2020-04-22"

keywords: view protected data

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

You can view associations between root keys and other cloud resources, such as Cloud Object Storage buckets or Cloud Databases deployments, by using the {{site.data.keyword.keymanagementservicelong_notm}} API.
{: shortdesc}

When you use a root key to protect at-rest data with envelope encryption, the cloud services that use the key can create a registration between the key and the resource that it protects. Registrations are associations between keys and cloud resources that help you get a full view of which encryption keys protect what data on {{site.data.keyword.cloud_notm}}.

| Benefit | Description |
| --- | --- |
| Centralized view of protected resources | As an administrator for your {{site.data.keyword.keymanagementserviceshort}} service instance, you want to quickly understand which cloud resources are protected by a root key. |
| Security and compliance | As a security admin, you need a way to determine the risk that's involved with [destroying a root key](/docs/key-protect?topic=key-protect-delete-keys). You want to examine which keys are actively protecting what data so that you can evaluate exposures based on your organization's security or compliance needs. |
{: caption="Table 1. Describes the benefits of key registration" caption-side="top"}

Key registration is an extra feature that's available only if the cloud service has enabled it as part of its integration with {{site.data.keyword.keymanagementserviceshort}}. To determine whether an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) supports key registration, refer to its service documentation for more information.
{: note}

This capability is currently available only by using the {{site.data.keyword.keymanagementserviceshort}} API. To find out more, check out the [{{site.data.keyword.keymanagementserviceshort}} API reference](/apidocs/key-protect#list-registrations-for-a-key){:external}.
{: preview}

## Viewing protected resources
{: #view-protected-resources-api}

You can browse the registrations that are available between your {{site.data.keyword.keymanagementserviceshort}} keys and cloud resources by using the {{site.data.keyword.keymanagementserviceshort}} API.

For example, when you call `GET api/v2/keys/{id}/registrations`, {{site.data.keyword.keymanagementserviceshort}} returns details about the key registration. The following JSON output represents a registration between a key and a cloud resource.

```json
{
  "metadata": {
      "collectionType": "application/vnd.ibm.kms.registration+json",
      "collectionTotal": 1
  },
  "resources": [
    {
      "keyId": "string",
      "resourceCrn": "crn:v1:bluemix:public:<service-name>:<region>:a/<account-id>:<service-instance>:bucket:<bucket-name>",
      "createdBy": "string",
      "creationDate": "2010-01-12T05:23:19+0000",
      "updatedBy": "string",
      "lastUpdated": "2010-01-12T05:23:19+0000",
      "description": "string",
      "preventKeyDeletion": true,
      "keyVersion": {
          "id": "string",
          "creationDate": "2010-01-12T05:23:19+0000"
      }
    }
  ]
}
```
{: screen}

The following table describes the properties of a registration.

| Parameter | Description |
| ---- | ---- |
| `keyID` | The ID that identifies the root key that is associated with the cloud resource. |
| `resourceCrn` | The Cloud Resource Name (CRN) that represents the cloud resource, such as a Cloud Object Storage bucket, that is associated with the key. |
| `createdBy` | The unique identifier for the resource that created the registration. |
| `creationDate` | The date the registration was created. |
| `updatedBy` | The unique identifier for the resource that updated the registration. |
| `lastUpdatedDate` | The date the registration was created. |
| `description` | A description for the registration.|
| `preventKeyDeletion` | A boolean that determines whether {{site.data.keyword.keymanagementserviceshort}} must prevent deletion of the root key. If `true`, the associated resource is non-erasable due to a retention policy, and the {{site.data.keyword.keymanagementserviceshort}} key that is encrypting the resource cannot be deleted. |
| `keyVersion` | The version of the root key that's protecting the cloud resource.|
{: caption="Table 2. Properties that are associated with a registration" caption-side="top"}

### Listing registrations for a specific root key
{: #view-protected-resources-key-id}

You can retrieve the registration details that are associated with a specific root key by making a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. View the registrations that are associated with a root key by running the following cURL command.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/registrations \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 3. Describes the variables that are needed to list all registrations that are associated with a root key.</caption>
    </table>

    A successful `GET api/v2/keys/<key_ID>/registrations` request returns a collection of registrations that are mapped to the specified key ID.

    ```json
    {
      "metadata": {
          "collectionType": "application/vnd.ibm.kms.registration+json",
          "collectionTotal": 2
      },
      "resources": [
        {
          "keyId": "string",
          "resourceCrn": "crn:v1:bluemix:public:cloud-object-storage:global:a/<account-id>:<service-instance>:bucket:<bucket-name>",
          "createdBy": "string",
          "creationDate": "2010-01-12T05:23:19+0000",
          "updatedBy": "string",
          "lastUpdated": "2010-01-12T05:23:19+0000",
          "description": "string",
          "preventKeyDeletion": true,
          "keyVersion": {
              "id": "string",
              "creationDate": "2010-01-12T05:23:19+0000"
          }
        },
        {
          "keyId": "string",
          "resourceCrn": "crn:v1:bluemix:public:cloud-object-storage:global:a/<account-id>:<service-instance>:bucket:<other-bucket-name>",
          "createdBy": "string",
          "creationDate": "2010-01-12T05:23:19+0000",
          "updatedBy": "string",
          "lastUpdated": "2010-01-12T05:23:19+0000",
          "description": "string",
          "preventKeyDeletion": true,
          "keyVersion": {
              "id": "string",
              "creationDate": "2010-01-12T05:23:19+0000"
          }
        }
      ]
    }
    ```
    {:screen}

    The `resourceCrn` value represents the unique identifier of the cloud resource that is encrypted by `keyId`. The metadata that is associated with the registration, such as its creation date, is also returned in the response body.

    By default, `GET api/v2/keys/registrations` returns the first 200 registrations, but you can adjust this limit by using the `limit` parameter at query time.
    {: note}

### Listing registrations for any root key
{: #view-protected-resources-any-key}

You can also retrieve a list of registrations that are associated with any cloud resource by making a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations?urlEncodedResourceCRNQuery=<url_encoded_CRN_query>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. View the registrations that match a CRN query that you specify by running the following cURL command.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys/registrations?urlEncodedResourceCRNQuery=<url_encoded_CRN_query> \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>url_encoded_CRN_query</varname></td>
        <td>
          <p>Filters for resources that are associated with a specified [Cloud Resource Name (CRN)](/docs/resources?topic=resources-crn#crn) by using URL encoded
        wildcard characters (`*`). The parameter should contain all CRN segments and must be URL encoded.</p>
          <p>To view examples, see [CRN query examples](#crn-query-examples).</p>
        </td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 4. Describes the variables that are needed to list registrations by CRN query.</caption>
    </table>

#### CRN query examples
{: #crn-query-examples}

Use URL encoded CRN queries that contain all CRN segments. To learn more about CRN segments and format, see [Cloud Resource Names](/docs/resources?topic=resources-crn).

When an integrated service calls the {{site.data.keyword.keymanagementserviceshort}} APIs, {{site.data.keyword.keymanagementserviceshort}} replaces the given CRN query (up to the `service-instance` segment) with the CRN of the calling service. This means that the services that use {{site.data.keyword.keymanagementserviceshort}} to associate keys with resources on your behalf can only view or query for CRNs that match the first 8 segments of their service CRN.
{: note}

- To search for the existence of a CRN segment, use a colon followed by an asterisk (`*`).

    ```
    crn:v1:bluemix:public:databases-for-redis:us-south:a/274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7:*:*
    ```
    {: screen}

    This query returns Databases for Redis registrations that are associated with all resource types and names for deployment ID _29caf0e7-120f-4da8-9551-3abf57ebcfc7_.


- To search for a CRN segment that's prefixed by `<string>`, use a colon followed by `<string>*` on the last segment of the CRN query.

  ```
  crn:v1:bluemix:public:cloud-object-storage:global:a/e1bb63d6a20dc57c87501ac4c4c99dcb:*:bucket:prod*
  ```
  {: screen}
  This query returns all Cloud Object Storage bucket registrations within account _e1bb63d6a20dc57c87501ac4c4c99dcb_ that are prefixed by `prod`.

  ```
  crn:v1:bluemix:public:databases-for-postgresql:us-south:a/e1bb63d6a20dc57c87501ac4c4c99dcb:76b98bfd-f730-47b8-b163-515187e070a7:*:<string>*
  ```
  {: screen}
  This query returns all Cloud Databases registrations for deployment ID _76b98bfd-f730-47b8-b163-515187e070a7_ that are prefixed by `<string>`.

The following tables provides a list of CRN query examples before and after URL encoding. To view the URL encoded values, click the **URL encoded** tab.

| Value|
| ---- |
|`crn:v1:bluemix:public:databases-for-redis:us-south:a/274074dce64e9c423ffc238516c755e1:29caf0e7-120f-4da8-9551-3abf57ebcfc7:*:*` |
|`crn:v1:bluemix:public:cloud-object-storage:global:a/e1bb63d6a20dc57c87501ac4c4c99dcb:*:bucket:prod*`  |
|`crn:v1:bluemix:public:cloudantnosqldb:us-south:a/f586c28d154d4c65a4a4a34cf75f55d0:94255ea3-af1c-41b7-9805-61f775e20702:*:prod*`. |
{: caption="Table 5. CRN query examples" caption-side="top"}
{: #table-5}
{: tab-title="Original"}
{: class="simple-tab-table"}

| Value |
| ---- |
|`crn%3Av1%3Abluemix%3Apublic%3Adatabases-for-redis%3Aus-south%3Aa%2F274074dce64e9c423ffc238516c755e1%3A29caf0e7-120f-4da8-9551-3abf57ebcfc7%3A*%3A*` |
| `crn%3Av1%3Abluemix%3Apublic%3Acloud-object-storage%3Aglobal%3Aa%2Fe1bb63d6a20dc57c87501ac4c4c99dcb%3A*%3Abucket%3Aprod*`  |
|`crn%3Av1%3Abluemix%3Apublic%3Acloudantnosqldb%3Aus-south%3Aa%2Ff586c28d154d4c65a4a4a34cf75f55d0%3A94255ea3-af1c-41b7-9805-61f775e20702%3A%2A%3Aprod%2A` |
{: caption="Table 6. CRN query examples" caption-side="top"}
{: #table-6}
{: tab-title="URL encoded"}
{: class="simple-tab-table"}

## What's next
{: #view-protected-resources-next-steps}

To find out more about viewing registrations, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](/apidocs/key-protect){: external}.




