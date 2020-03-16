---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-14"

keywords: delete key, delete key API examples

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

# Deleting keys using a single authorization
{: #delete-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to delete an encryption key and its contents, if you are a manager for your {{site.data.keyword.keymanagementserviceshort}} service instance.
{: shortdesc}

When you delete a key, you shred its contents and associated data. Any data that is encrypted by the key becomes inaccessible. [Destroying resources](/docs/key-protect?topic=key-protect-security-and-compliance#data-deletion) is not recommended for production environments, but might be useful for temporary environments such as testing or QA.
{: important}

Keep in mind the following considerations before you delete a key:

- {{site.data.keyword.keymanagementserviceshort}} blocks the deletion of any key that's actively protecting a cloud resource. Before you delete a key, [review the resources](/docs/key-protect?topic=key-protect-view-protected-resources) that are associated with the key.
- You can [force deletion on a key](#delete-key-force) that's protecting a cloud resource. However, the action won't succeed if the key's associated resource is non-erasable due to a retention policy. You can verify whether a key is associated with a non-erasable resource by [checking the registration details](/docs/key-protect?topic=key-protect-view-protected-resources#view-protected-resources-api) for the key.

## Deleting keys in the console
{: #delete-key-gui}

By default, {{site.data.keyword.keymanagementserviceshort}} requires one authorization to delete a key. If you prefer to delete your encryption keys by using a graphical interface, you can use the {{site.data.keyword.cloud_notm}} console.

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys), complete the following steps to delete a key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the application details page, use the **Keys** table to browse the keys in your service.
5. Click the ⋯ icon to open a list of options for the key that you want to delete.
6. From the options menu, click **Delete key** and confirm the key deletion in the next screen.

After you delete a key, the key transitions to the _Destroyed_ state. Keys in this state are no longer recoverable. Metadata that is associated with the key, such as the key's deletion date, is kept in the {{site.data.keyword.keymanagementserviceshort}} database.

## Deleting keys with the API
{: #delete-key-api}

By default, {{site.data.keyword.keymanagementserviceshort}} requires one authorization to delete a key. You can delete a key and its contents by making a `DELETE` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```

This action won't succeed if the key is actively protecting one or more cloud resources. You can [review the resources](/docs/key-protect?topic=key-protect-view-protected-resources) that are associated with the key, or [use the `force` parameter](#delete-key-force) at query time to delete the key. 
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you want to delete.

    You can find the ID for a key in your service instance by [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys), or by accessing the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following cURL command to delete the key and its contents.

    ```cURL
    curl -X DELETE \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'prefer: <return_preference>'
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
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the key that you would like to delete.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>return_preference</varname></td>
        <td><p>A header that alters server behavior for <code>POST</code> and <code>DELETE</code> operations.</p><p>When you set the <em>return_preference</em> variable to <code>return=minimal</code>, the service returns a successful deletion response. When you set the variable to <code>return=representation</code>, the service returns both the key material and the key metadata.</p></td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to delete keys with the {{site.data.keyword.keymanagementserviceshort}} API.</caption>
    </table>

    If the `return_preference` variable is set to `return=representation`, the details of the `DELETE` request are returned in the response entity-body. <!--After you delete a key, it enters the `Deactivated` key state. After 24 hours, if a key is not reinstated, the key transitions to the `Destroyed` state. The key contents are permanently erased and no longer accessible.--> The following JSON object shows an example returned value.
    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "acc29d78-c7d4-4b3f-be3a-4cb8d768be6c",
          "name": "test-root-key",
          "state": 5,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:acc29d78-c7d4-4b3f-be3a-4cb8d768be6c",
          "imported": false,
          "creationDate": "2020-03-10T20:41:27Z",
          "createdBy": "IBMid-503CKNRHR7",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-16T20:41:27Z",
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": true,
          "deletionDate": "2020-03-16T21:46:53Z",
          "deletedBy": "IBMid-503CKNRHR7"
        }
      ]
    }
    ```
    {: screen}

    For a detailed description of the available parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](https://{DomainName}/apidocs/key-protect){: external}.

### Using the `force` query parameter
{: #delete-key-force}

{{site.data.keyword.keymanagementserviceshort}} blocks the deletion of a key that's protecting a cloud resource, such as a Cloud Object Storage bucket. You can force delete a key and its contents by making a `DELETE` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?force=true
```
This action won't succeed if the key is protecting a resource that's non-erasable due to a retention policy. You can verify whether a key is associated with a non-erasable resource by [checking the registration details](/docs/key-protect?topic=key-protect-view-protected-resources) for the key. Then, you must contact an account owner to remove the retention policy on each resource that is associated with the key before you can delete the key.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you want to force delete.

    You can retrieve the ID for a specified key by making a `GET /v2/keys/` request, or by viewing your keys in the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following cURL command to force delete the key and its contents.

    ```cURL
    curl -X DELETE \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?force=true \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'prefer: <return_preference>'
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
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the key that you would like to delete.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>return_preference</varname></td>
        <td><p>A header that alters server behavior for <code>POST</code> and <code>DELETE</code> operations.</p><p>When you set the <em>return_preference</em> variable to <code>return=minimal</code>, the service returns a successful deletion response. When you set the variable to <code>return=representation</code>, the service returns both the key material and the key metadata.</p></td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to delete keys with the {{site.data.keyword.keymanagementserviceshort}} API.</caption>
    </table>

    If the `return_preference` variable is set to `return=representation`, the details of the `DELETE` request are returned in the response entity-body. <!--After you delete a key, it enters the `Deactivated` key state. After 24 hours, if a key is not reinstated, the key transitions to the `Destroyed` state. The key contents are permanently erased and no longer accessible.--> The following JSON object shows an example returned value.
    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "acc29d78-c7d4-4b3f-be3a-4cb8d768be6c",
          "name": "test-root-key",
          "state": 5,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:acc29d78-c7d4-4b3f-be3a-4cb8d768be6c",
          "imported": false,
          "creationDate": "2020-03-10T20:41:27Z",
          "createdBy": "IBMid-503CKNRHR7",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-16T20:41:27Z",
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": true,
          "deletionDate": "2020-03-16T21:46:53Z",
          "deletedBy": "IBMid-503CKNRHR7"
        }
      ]
    }
    ```
    {: screen}

    For a detailed description of the available parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](https://{DomainName}/apidocs/key-protect){: external}.

