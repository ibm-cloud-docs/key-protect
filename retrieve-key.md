---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

keywords: get key, get encryption key, view encryption key, retrieve encryption key, API examples

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

# Retrieving a key
{: #retrieve-key}

You can retrieve the key material for a key by using {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

If you have a _Writer_ or _Manager_ access policy, you can retrieve the contents of a standard key, such as its key material and policy details. 

[Root keys](#x6946961){:term} stay within the bounds of a hardware security module. The key material for a root key cannot be retrieved.
{: note}

## Retrieving a key with the API
{: #get-key-api}

To view detailed information about a specific key, you can make a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you would like to access or manage.

    The ID value is used to access detailed information about the key, such as the key material itself. You can retrieve the ID for a specified key by making a `GET /v2/keys` request, or by accessing the {{site.data.keyword.keymanagementserviceshort}} GUI.

3. Run the following cURL command to get details about your key and the key material.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID> \
      -H 'accept: application/vnd.ibm.kms.key+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>'
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
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. See <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a> for more information.</td>
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
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The identifier for the key that you retrieved in [step 1](#retrieve-keys-api).</td>
      </tr>
      <caption style="caption-side:bottom;">Table 4. Describes the variables that are needed to view a specified key with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `GET api/v2/keys/<key_ID>` response returns details about your key and the key material. The following JSON object shows an example returned value for a standard key.

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "6efbc310-63a4-46ee-ae73-cb55ac072039",
          "name": "test-standard-key",
          "state": 1,
          "extractable": true,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:6efbc310-63a4-46ee-ae73-cb55ac072039",
          "imported": false,
          "creationDate": "2020-03-12T03:50:12Z",
          "createdBy": "IBMid-503CKNRHR7",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-12T03:50:12Z",
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": false,
          "payload": "tO7DWNXERuBjgc5UDSBB04F64xEI6c9zGe8l2VfBRyQ="
        }
      ]
    }
    ```
    {: screen}

    The following JSON object shows an example returned value for a root key.

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "9a4ab000-3d86-421c-adad-8be5944a3d9d",
          "name": "test-root-key",
          "state": 1,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:9a4ab000-3d86-421c-adad-8be5944a3d9d",
          "imported": false,
          "creationDate": "2020-03-05T16:28:38Z",
          "createdBy": "IBMid-503CKNRHR7",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-05T16:39:25Z",
          "keyVersion": {
            "id": "49198706-733f-481b-a5f0-49e8bf853b3c",
            "creationDate": "2020-03-05T16:39:25Z"
          },
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": false
        }
      ]
    }
    ```
    {:screen}

    The `payload` or key material for a root key stays within the bounds of a hardware security module and cannot be retrieved.
    {: note}

    For a detailed description of the response parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](https://{DomainName}/apidocs/key-protect){: external}.
