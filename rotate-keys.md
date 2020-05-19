---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-20"

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

You can rotate your root keys manually by using
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

When you rotate your root key, you replace the key with new key material. This
process creates a new key version that you can use for
[rewrapping or reencrypting your data](/docs/key-protect?topic=key-protect-rewrap-keys).

To learn how key rotation helps you meet industry standards and cryptographic
best practices, see
[Rotating your encryption keys](/docs/key-protect?topic=key-protect-key-rotation).

Rotation is available only for root keys. To learn more about your key rotation
options in {{site.data.keyword.keymanagementserviceshort}}, check out
[Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Rotating root keys in the console
{: #rotate-key-gui}

If you prefer to rotate your root keys by using a graphical interface, you can
use the {{site.data.keyword.cloud_notm}} console.

[After you create root key](/docs/key-protect?topic=key-protect-create-root-keys),
complete the following steps to rotate the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your
provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the application details page, use the **Keys** table to browse the keys in
your service.
5. Click the ⋯ icon to open a list of options for the key that you want to
rotate.
6. From the options menu, click **Rotate key**.

    If you initially provided the key material for the key, specify the
    following details:

    <table>
      <tr>
        <th>Setting</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          Key material
        </td>
        <td>
          <p>
            The new base64 encoded key material that you want to store and
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
        Table 1. Describes the <b>Rotate key</b> settings
      </caption>
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

    You can find the ID for a key in your service instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
          <p>
            <strong>Required.</strong> The unique identifier for the root key
            that you want to rotate.
          </p>
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
            token, including the Bearer value, in the cURL request.
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
            The new base64 encoded key material that you want to store and
            manage in the service. This value is required if you initially
            imported the key material when you added the key to the service.
          </p>
          <p>
            To rotate a key that was initially generated by
            {{site.data.keyword.keymanagementserviceshort}}, omit the
            <code>payload</code> attribute and pass an empty request
            entity-body. To rotate an imported key, provide a key material that
            meets the following requirements:
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
        Table 1. Describes the variables that are needed to rotate a specified
        key in {{site.data.keyword.keymanagementserviceshort}}.
      </caption>
    </table>

    A successful rotation request returns an HTTP `204 No Content` response,
    which indicates that your root key was replaced by new key material.

4. Optional: Verify that the key was rotated by running the following call to
browse the keys in your {{site.data.keyword.keymanagementserviceshort}} service
instance.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/metadata \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Review the `lastRotateDate` and `keyVersion` values in the response
    entity-body to inspect the date and time that your key was last rotated.

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
          "name": "test-root-key",
          "state": 1,
          "extractable": false,
          "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
          "imported": false,
          "creationDate": "2020-03-12T03:50:12Z",
          "createdBy": "...",
          "algorithmType": "AES",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
          },
          "algorithmBitSize": 256,
          "algorithmMode": "CBC_PAD",
          "lastUpdateDate": "2020-03-12T03:50:12Z",
          "lastRotateDate": "2020-03-12T03:49:01Z",
          "keyVersion": {
            "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "creationDate": "2020-03-12T03:50:12Z",
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

    The `keyVersion` attribute contains identifying information that describes
    the latest version of the root key.

    You can also list the versions that are available for the key by using the
    {{site.data.keyword.keymanagementserviceshort}} API. To learn more, see
    [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
    {: tip}

### Using an import token to rotate a key
{: #rotate-keys-secure-api}

If you initially imported a root key by using an import token, you can rotate
the key by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To rotate a key, you must be assigned a _Writer_ or _Manager_ access policy
    for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the ID of the key that you want to rotate.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    request, or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. [Create and retrieve an import token](/docs/key-protect?topic=key-protect-create-import-tokens).

4. Use the import token to encrypt the key material that you want to use to
rotate the existing key.

    To learn how to use an import token, check out
    [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).

5. Replace the existing key with new key material by running the following cURL
command.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -d '{
        "type": "application/vnd.ibm.kms.key+json",
        "name": "<key_alias>",
        "description": "<key_description>",
        "extractable": <key_type>,
        "payload": "<encrypted_key>",
        "encryptionAlgorithm": "RSAES_OAEP_SHA_256",
        "encryptedNonce": "<encrypted_nonce>",
        "iv": "<iv>"
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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
          <p>
            <strong>Required.</strong> The unique identifier for the key that
            you want to rotate.
          </p>
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
            token, including the Bearer value, in the cURL request.
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
          <varname>key_alias</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> A unique, human-readable name for easy
            identification of your key. To protect your privacy, do not store
            your personal data as metadata for your key.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_description</varname>
        </td>
        <td>
          <p>
            An extended description of your key. To protect your privacy, do not
            store your personal data as metadata for your key.
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
          <varname>key_type</varname>
        </td>
        <td>
          <p>
            A boolean value that determines whether the key material can leave
            the service.
          </p>
          <p>
            When you set the <code>extractable</code> attribute to
            <code>false</code>, the service designates the key as a root key
            that you can use for <code>wrap</code> or <code>unwrap</code>
            operations.
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
            [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce){: external}.
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
            [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys#tutorial-import-encrypt-nonce){: external}.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to restore keys with
        the {{site.data.keyword.keymanagementserviceshort}} API.
      </caption>
    </table>

    A successful rotation request returns an HTTP `204 No Content` response,
    which indicates that your root key was replaced by new key material.

6. Optional: Verify that the key was rotated by retrieving details about the
key.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
      -H 'accept: application/vnd.ibm.kms.key+json'
    ```
    {: codeblock}

    Review the `lastRotateDate` and `keyVersion` values in the response
    entity-body to inspect the date and time that your key was last rotated.

    You can also list the versions that are available for the key by using the
    {{site.data.keyword.keymanagementserviceshort}} API. To learn more, see
    [Viewing key versions](/docs/key-protect?topic=key-protect-view-key-versions).
    {: tip}
