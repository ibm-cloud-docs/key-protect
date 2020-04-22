---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-22"

keywords: rewrap key, reencrypt data encryption key, rewrap API examples

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

# Rewrapping keys
{: #rewrap-keys}

Reencrypt your data encryption keys by using the {{site.data.keyword.keymanagementservicelong}} API.
{: shortdesc}

When you [rotate a root key in {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-key-rotation), new cryptographic key material becomes available for protecting the data encryption keys (DEKs) that are associated with the root key. With the rewrap API, you can reencrypt or rewrap your DEKs without exposing the keys in their plaintext form.

To learn how envelope encryption helps you control the security of at-rest data in the cloud, see [Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## Rewrapping keys by using the API
{: #rewrap-key-api}

You can reencrypt a specified data encryption key (DEK) with a root key that you manage in {{site.data.keyword.keymanagementserviceshort}}, without exposing the DEK in its plaintext form.

Rewrapping keys works by combining `unwrap` and `wrap` calls to the service. For example, you can emulate a `rewrap` operation by first calling the `unwrap` API to access a DEK, and then calling the `wrap` API to reencrypt the DEK by using the newest root key material.
{: note}

[After you rotate a root key in the service](/docs/key-protect?topic=key-protect-rotate-keys), rewrap a data encryption key that is associated with the root key by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rewrap
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).
2. Copy the ID of the rotated root key that you used to perform the initial wrap request.

    You can retrieve the ID for a key by making a `GET api/v2/keys` request, or by viewing your keys in the {{site.data.keyword.keymanagementserviceshort}} GUI.
3. Copy the `ciphertext` value that was returned during the latest wrap request.
4. Rewrap the key with the latest root key material by running the following cURL command.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rewrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
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
        <td><strong>Required.</strong> The unique identifier for the root key that you used for the initial wrap request.</td>
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
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>Required.</strong> The <code>ciphertext</code> value that was returned by the original wrap operation.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supplied AAD for the initial wrap call, you must specify the same AAD during the subsequent unwrap or rewrap calls.<br></br>Important: The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap or rewrap requests.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to rewrap keys in {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    The newly wrapped data encryption key, original key version (`keyVersion`) that is associated with the supplied ciphertext and latest key version (`rewrappedKeyVersion`) associated with the new ciphertext is returned in the response entity-body. The following JSON object shows an example returned value.

    ```json
    {
      "ciphertext": "ciphertext-goes-here",
      "keyVersion": {
        "id": "..."
      },
      "rewrappedKeyVersion": {
        "id": "..."
      }
    }
    ```
    {:screen}

    Store and use the new `ciphertext` value for future envelope encryption operations so that your data is protected by the latest root key.
