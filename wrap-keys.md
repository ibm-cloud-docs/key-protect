---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-14"

keywords: wrap key, encrypt data encryption key, protect data encryption key, envelope encryption API examples

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Wrapping keys
{: #wrap-keys}

You can manage and protect your encryption keys with a root key by using the {{site.data.keyword.keymanagementservicelong}} API, if you are a privileged user.
{: shortdesc}

When you wrap a data encryption key (DEK) with a root key, {{site.data.keyword.keymanagementserviceshort}} combines the strength of multiple algorithms to protect the privacy and the integrity of your encrypted data.  

To learn how key wrapping helps you control the security of at-rest data in the cloud, see [Protecting data with envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Wrapping keys by using the API
{: #wrap-key-api}

You can protect a specified data encryption key (DEK) with a root key that you manage in {{site.data.keyword.keymanagementserviceshort}}.

When you supply a root key for wrapping, ensure that the root key is 128, 192, or 256 bits so that the wrap call can succeed. If you create a root key in the service, {{site.data.keyword.keymanagementserviceshort}} generates a 256-bit key from its HSMs, supported by the AES-GCM algorithm.
{: note}

[After you designate a root key in the service](/docs/services/key-protect?topic=key-protect-create-root-keys), you can wrap a DEK with advanced encryption by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copy the key material of the DEK that you want to manage and protect.

    If you have manager or writer privileges for your {{site.data.keyword.keymanagementserviceshort}} service instance, [you can retrieve the key material for a specific key by making a `GET /v2/keys/<key_ID>` request](/docs/services/key-protect?topic=key-protect-view-keys#view-keys-api	).

3. Copy the ID of the root key that you want to use for wrapping.

4. Run the following cURL command to protect the key with a wrap operation.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
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
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the root key that you want to use for wrapping.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>data_key</varname></td>
        <td>The key material of the DEK that you want to manage and protect. The <code>plaintext</code> value must be base64 encoded. To generate a new DEK, omit the <code>plaintext</code> attribute. The service generates a random plaintext (32 bytes), wraps that value, and then returns both the generated and wrapped values in the response.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supply AAD when you make a wrap call to the service, you must specify the same AAD during the subsequent unwrap call.<br></br>Important: The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap requests.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to wrap a specified key in {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Your wrapped data encryption key, containing the base64 encoded key material, is returned in the response entity-body. The following JSON object shows an example returned value.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    If you omit the `plaintext` attribute when you make the wrap request, the service returns both the generated data encryption key (DEK) and the wrapped DEK in base64 encoded format.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    The <code>plaintext</code> value represents the unwrapped DEK, and <code>ciphertext</code> value represents the wrapped DEK.
    
    If you want {{site.data.keyword.keymanagementserviceshort}} to generate a new data encryption key (DEK) on your behalf, you can also pass in an empty body on a wrap request. Your generated DEK, containing the base64 encoded key material, is returned in the response entity-body, along with the wrapped DEK.
    {: tip}
    