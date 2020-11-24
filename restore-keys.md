---

copyright:
  years: 2020
lastupdated: "2020-11-24"

keywords: restore key, restore a deleted key, re-import a key

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

# Restoring keys
{: #restore-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to restore a
previously deleted root key and access its associated data on the cloud.
{: shortdesc}

As an admin, you might need to restore a root key that you imported to
{{site.data.keyword.keymanagementserviceshort}} so that you can access data that
the key previously protected. When you restore a key, you move the key from the
_Destroyed_ to the _Active_ key state, and you restore access to any data that
was previously encrypted with the key.

At this time, you are only able to restore root keys. Support is not available
for restoring standard keys.

You can restore a deleted key within 30 days of its deletion. This capability is
available only for root keys that were previously imported to the service.
{: note}

## Restoring a deleted key with the console
{: #restore-ui}

If you prefer to restore your root key by using a graphical interface, you can
use the IBM Cloud console.

[After you import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys) and [delete](/docs/key-protect?topic=key-protect-delete-keys) a root key,
complete the following steps to restore the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, click the filter icon and select the
   dropdown from the **Status** menu.

5. Select the **Destroyed** state.

6. Click the **Apply** button.

7. Click the ⋯ icon to open a list of options for the key that you want to
   delete.

8. Click the **Restore Key** button to open a tab and enter the key ID and
   original key material that was associated with the deleted key.

9. Click **Restore Key** button.

10. Confirm the key was restored in the updated **Keys** table.

## Restoring a deleted key with the API
{: #restore-api}

Restore a previously imported root key by making a `POST` call to the following
endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/restore
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To restore a key, you must be assigned a _Manager_ access policy for the
    instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the ID of the key that you want to restore.

    You can retrieve the ID for a specified key by making a
    [list keys request](/apidocs/key-protect#list-keys){: external}
    request, or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to restore the key and regain access to its
   associated data.

   You cannot restore a key that has an expiration date that is current or in
   the past.
   {: important}

   You must wait 30 seconds after deleting a key before you are able to restore
   it.
   {: note}

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/restore" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.key+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "payload": "<key_material>"
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as
            <code>us-south</code> or <code>eu-gb</code>, that represents the
            geographic area where your
            {{site.data.keyword.keymanagementserviceshort}} instance
            resides.
          </p>
          <p>
            For more information, see
            [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_ID</varname>
        </td>
        <td>
          <strong>Required.</strong> The unique identifier for the key that you
          want to restore.
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the <code>curl</code> request.
          </p>
          <p>
            For more information, see
            [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to
            your {{site.data.keyword.keymanagementserviceshort}} service
            instance.
          </p>
          <p>
            For more information, see
            [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_material</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The base64 encoded key material, such as
            an existing such as an existing root key, that you want to store and
            manage in the service.
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be
                encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to restore keys with
        the {{site.data.keyword.keymanagementserviceshort}} API.
      </caption>
    </table>

    A successful restore request returns an HTTP `201 Created` response, which
    indicates that the key was restored to the _Active_ key state and is now
    available for encrypt and decrypt operations. All attributes and policies
    that were previously associated with the key are also restored.

    You will have access to data associated with the key as soon as the key is
    restored.
    {: note}

4. Optional: Verify that the key was restored by retrieving details about the
   key.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata" \
        -H "accept: application/vnd.ibm.kms.key+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Review the `state` field in the response body to verify that the key
    transitioned to the _Active_ key state. The following JSON output shows the
    metadata details for an _Active_ key.

    The integer mapping for the _Active_ key state is 1. Key States are based on
    NIST SP 800-57.
    {: note}

    ```json
    {
        "metadata": {
            "collectionType": "application/vnd.ibm.kms.key+json",
            "collectionTotal": 1
        },
        "resources": [
            {
                "type": "application/vnd.ibm.kms.key+json",
                "id": "02fd6835-6001-4482-a892-13bd2085f75d",
                "name": "...",
                "description": "...",
                "tags": [
                    "..."
                ],
                "state": 1,
                "extractable": false,
                "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
                "imported": true,
                "creationDate": "2020-03-10T20:41:27Z",
                "createdBy": "...",
                "algorithmType": "AES",
                "algorithmMetadata": {
                    "bitLength": "128",
                    "mode": "CBC_PAD"
                },
                "algorithmBitSize": 128,
                "algorithmMode": "CBC_PAD",
                "lastUpdateDate": "2020-03-16T20:41:27Z",
                "keyVersion": {
                    "id": "30372f20-d9f1-40b3-b486-a709e1932c9c",
                    "creationDate": "2020-03-12T03:37:32Z"
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

### Using an import token to restore a key
{: #restore-keys-secure-api}

If you initially used an import token to import the root key, you can restore
the key by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/restore
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To restore a key, you must be assigned a _Manager_ access policy for the
    instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the ID of the key that you want to restore.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    request, or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. [Create and retrieve an import token](/docs/key-protect?topic=key-protect-create-import-tokens).

4. Use the import token to encrypt the key that you want to restore.

    To learn how to use an import token, check out
    [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).

5. Restore the key and regain access to its associated data by running the
   following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/restore" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.key+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "payload": "<encrypted_key>",
                        "encryptionAlgorithm": "RSAES_OAEP_SHA_256",
                        "encryptedNonce": "<encrypted_nonce>",
                        "iv": "<iv>"
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          <varname>region</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The region abbreviation, such as
            <code>us-south</code> or <code>eu-gb</code>, that represents the
            geographic area where your
            {{site.data.keyword.keymanagementserviceshort}} instance
            resides.
          </p>
          <p>
            For more information, see
            [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_ID</varname>
        </td>
        <td>
          <strong>Required.</strong> The unique identifier for the key that you
          want to restore.
        </td>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the <code>curl</code> request.
          <p>
          <p>
            For more information, see
            [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
          <p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>instance_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier that is assigned to
            your {{site.data.keyword.keymanagementserviceshort}} service
            instance.
          </p>
          <p>
            For more information, see
            [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>encrypted_key</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The encrypted key material that you want
            to store and manage in the service. The value must be base64
            encoded.
          </p>
          <p>
            Ensure that the key material meets the following requirements:
          </p>
          <p>
            <ul>
              <li>
                The key must be 128, 192, or 256 bits.
              </li>
              <li>
                The bytes of data, for example 32 bytes for 256 bits, must be
                encoded by using base64 encoding.
              </li>
            </ul>
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>encrypted_nonce</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The AES-GCM encrypted nonce that ensures
            that the bits you send as part of a request are exactly the same as
            what we receive. The nonce validates the key that you are restoring.
          </p>
          <p>
            To learn more, see
            [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>iv</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The initialization vector (IV) that is
            generated by the AES-GCM algorithm when you encrypt a nonce. This
            value is used to decode the key for storage in the
            {{site.data.keyword.keymanagementserviceshort}} system.
          </p>
          <p>
            To learn more, see
            [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce).
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 2. Describes the variables that are needed to restore keys via
        Import Token with the {{site.data.keyword.keymanagementserviceshort}}
        API.
      </caption>
    </table>

    A successful restore request returns an HTTP `201 Created` response, which
    indicates that the key was restored to the _Active_ key state and is now
    available for encrypt and decrypt operations. All attributes and policies
    that were previously associated with the key are also restored.

    You will have access to data associated with the key as soon as the key is
    restored.
    {: note}

6. Optional: Verify that the key was restored by retrieving details about the
   key.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata" \
        -H "accept: application/vnd.ibm.kms.key+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Review the `state` field in the response body to verify that the key
    transitioned to the _Active_ key state. The following JSON output shows the
    metadata details for an _Active_ key.

    The integer mapping for the _Active_ key state is 1. Key States are based on
    NIST SP 800-57.
    {: note}

    ```json
    {
        "metadata": {
            "collectionType": "application/vnd.ibm.kms.key+json",
            "collectionTotal": 1
        },
        "resources": [
            {
                "type": "application/vnd.ibm.kms.key+json",
                "id": "02fd6835-6001-4482-a892-13bd2085f75d",
                "name": "...",
                "description": "...",
                  "tags": [
                      "..."
                  ],
                "state": 1,
                "extractable": false,
                "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
                "imported": true,
                "creationDate": "2020-03-10T20:41:27Z",
                "createdBy": "...",
                "algorithmType": "AES",
                "algorithmMetadata": {
                    "bitLength": "128",
                    "mode": "CBC_PAD"
                },
                "algorithmBitSize": 128,
                "algorithmMode": "CBC_PAD",
                "lastUpdateDate": "2020-03-16T20:41:27Z",
                "keyVersion": {
                    "id": "30372f20-d9f1-40b3-b486-a709e1932c9c",
                    "creationDate": "2020-03-12T03:37:32Z"
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
