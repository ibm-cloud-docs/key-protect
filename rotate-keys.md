---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-14"

keywords: rotate encryption key, encryption key rotation, rotate key API examples 

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

# Manually rotating keys
{: #rotate-keys}

You can rotate your root keys manually by using {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

When you rotate your root key, you replace the key with new key material. This process creates a new key version that you can use for [rewrapping or reencrypting your data](/docs/key-protect?topic=key-protect-rewrap-keys).

To learn how key rotation helps you meet industry standards and cryptographic best practices, see [Rotating your encryption keys](/docs/key-protect?topic=key-protect-key-rotation).

Rotation is available only for root keys. To learn more about your key rotation options in {{site.data.keyword.keymanagementserviceshort}}, check out [Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Rotating root keys in the console
{: #rotate-key-gui}

If you prefer to rotate your root keys by using a graphical interface, you can use the {{site.data.keyword.cloud_notm}} console.

[After you create root key](/docs/key-protect?topic=key-protect-create-root-keys), complete the following steps to rotate the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the application details page, use the **Keys** table to browse the keys in your service.
5. Click the ⋯ icon to open a list of options for the key that you want to rotate.
6. From the options menu, click **Rotate key**.

    If you initially provided the key material for the key, specify the following details:

    <table>
      <tr>
        <th>Setting</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Key material</td>
        <td>
          <p>The new base64 encoded key material that you want to store and manage in the service.</p>
          <p>Ensure that the key material meets the following requirements:</p>
          <p>
            <ul>
              <li>The key must be 128, 192, or 256 bits.</li>
              <li>The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the <b>Rotate key</b> settings</caption>
    </table>

7.  When you are finished, click **Rotate key** to confirm. 

## Rotating root keys with the API
{: #rotate-key-api}

You can rotate a root key by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [Retrieve authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Copy the ID of the root key that you want to rotate.

    You can find the ID for a key in your service instance by [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys), or by accessing the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Replace the key with new key material by running the following cURL command.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <key_material>'
      }'
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
        <td><strong>Required.</strong> The unique identifier for the root key that you want to rotate.</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>The new base64 encoded key material that you want to store and manage in the service. This value is required if you initially imported the key material when you added the key to the service.</p>
          <p>To rotate a key that was initially generated by {{site.data.keyword.keymanagementserviceshort}}, omit the <code>payload</code> attribute and pass an empty request entity-body. To rotate an imported key, provide a key material that meets the following requirements:</p>
          <p>
            <ul>
              <li>The key must be 128, 192, or 256 bits.</li>
              <li>The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to rotate a specified key in {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    A successful rotation request returns an HTTP `204 No Content` response, which indicates that your root key was replaced by new key material. 

4. Optional: Verify that the key was rotated by running the following call to browse the keys in your {{site.data.keyword.keymanagementserviceshort}} service instance.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys/metadata \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}
  
    Review the `lastRotateDate` and `keyVersion` values in the response entity-body to inspect the date and time that your key was last rotated.

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "type": "application/vnd.ibm.kms.key+json",
          "id": "e71e0666-f3a7-4d57-b2ee-3040568f3d83",
          "name": "test-root-key",
          "state": 1,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:e71e0666-f3a7-4d57-b2ee-3040568f3d83",
          "imported": false,
          "creationDate": "2020-03-13T18:05:23Z",
          "createdBy": "IBMid-503CKNRHR7",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-16T21:02:32Z",
          "lastRotateDate": "2020-03-16T21:02:32Z",
          "keyVersion": {
            "id": "b0791e63-273c-4a38-8602-d49cd9ebb4d5",
            "creationDate": "2020-03-16T21:02:32Z"
          },
          "dualAuthDelete": {
            "enabled": false
          },
          "deleted": false
        }
      ]
    }
    ```
    {: screen}

    The `keyVersion` attribute contains identifying information that describes the latest version of the root key.

    You can also list the versions that are available for the key by using the {{site.data.keyword.keymanagementserviceshort}} API. To learn more, see [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
    {: tip}

