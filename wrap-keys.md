---

copyright:
  years: 2017, 2020, 2021
lastupdated: "2021-01-04"

keywords: wrap key, encrypt data encryption key, protect data encryption key, envelope encryption API examples

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

# Wrapping keys
{: #wrap-keys}

You can manage and protect your encryption keys with a root key by using the
{{site.data.keyword.keymanagementservicelong}} API.
{: shortdesc}

When you wrap a [data encryption key](#x4791827){: term} with a
[root key](#x6946961){: term}, {{site.data.keyword.keymanagementserviceshort}}
combines the strength of multiple algorithms to protect the privacy and the
integrity of your encrypted data.

To learn how key wrapping helps you control the security of at rest data in the
cloud, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
{: tip}

## Wrapping keys by using the API
{: #wrap-key-api}

You can protect a specified data encryption key (DEK) with a root key that you
manage in {{site.data.keyword.keymanagementserviceshort}}.

[After you designate a root key in the service](/docs/key-protect?topic=key-protect-create-root-keys),
you can wrap a DEK with advanced encryption by making a `POST` call to the
following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/wrap
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Copy the key material of the DEK that you want to manage and protect.

    If you have manager or writer privileges for your
    {{site.data.keyword.keymanagementserviceshort}} instance,
    [you can retrieve the key material for a specific key by making a `GET /v2/keys/<key_ID>` request](/docs/key-protect?topic=key-protect-view-keys#view-keys-api).

3. Copy the ID of the root key that you want to use for wrapping.

4. Run the following `curl` command to protect the key with a wrap operation.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/wrap" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "correlation-id: <correlation_ID>" \
        -d '{
                "plaintext": "<data_key>",
                "aad": [
                    "<additional_data>",
                    "<additional_data>"
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
          <strong>Required.</strong> The unique identifier for the root key that
          you want to use for wrapping.
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
          <varname>key_ring_ID</varname>
        </td>
        <td>
          <p>
            <strong>Optional.</strong> The unique identifier of the key ring that the key belongs to. 
            If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key 
            in every key ring associated with the specified instance. It is recommended to specify 
            the key ring ID for a more optimized request.

            Note: The key ring ID of keys that are created without an `x-kms-key-ring` 
            header is: default.
          </p>
          <p>
            For more information, see
            [Managing key rings](docs/key-protect?topic=key-protect-managing-key-rings).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>correlation_ID</varname>
        </td>
        <td>
          The unique identifier that is used to track and correlate
          transactions.
        </td>
      </tr>

      <tr>
        <td>
          <varname>data_key</varname>
        </td>
        <td>
          <p>
            The key material of the DEK that you want to manage and protect. The
            <code>plaintext</code> value must be base64 encoded.
          </p>
          <p>
            For more information on encoding your key material, see
            [Encoding your key material](/docs/key-protect?topic=key-protect-import-root-keys#how-to-encode-root-key-material).
          </p>
          <p>
            To generate a new DEK, omit the <code>plaintext</code> attribute.
            The service generates a random plaintext (32 bytes), wraps that
            value, and then returns both the generated and wrapped values in the
            response. The generated and wrapped values are base64 encoded and
            you will need to decode them in order to decrypt the keys.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>additional_data</varname>
        </td>
        <td>
          The additional authentication data (AAD) that is used to further
          secure the key. Each string can hold up to 255 characters. If you
          supply AAD when you make a wrap call to the service, you must specify
          the same AAD during the subsequent unwrap call.<br></br>Important:
          The {{site.data.keyword.keymanagementserviceshort}} service does not
          save additional authentication data. If you supply AAD, save the data
          to a secure location to ensure that you can access and provide the
          same AAD during subsequent unwrap requests.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to wrap a specified key
        in {{site.data.keyword.keymanagementserviceshort}}.
      </caption>
    </table>

    Your wrapped data encryption key, containing the base64 encoded key
    material, is returned in the response entity-body. The response body also
    contains the ID of the key version that was used to wrap the supplied
    plaintext. The following JSON object shows an example returned value.

    ```json
    {
        "ciphertext": "eyJjaXBoZXJ0ZXh0IjoiYmFzZTY0LWtleS1nb2VzLWhlcmUiLCJpdiI6IjRCSDlKREVmYU1RM3NHTGkiLCJ2ZXJzaW9uIjoiNC4wLjAiLCJoYW5kbGUiOiJ1dWlkLWdvZXMtaGVyZSJ9",
        "keyVersion": {
            "id": "02fd6835-6001-4482-a892-13bd2085f75d"
        }
    }
    ```
    {: screen}

    If you omit the `plaintext` attribute when you make the wrap request, the
    service returns both the generated data encryption key (DEK) and the wrapped
    DEK in base64 encoded format.

    ```json
    {
        "plaintext": "Rm91ciBzY29yZSBhbmQgc2V2ZW4geWVhcnMgYWdv",
        "ciphertext": "eyJjaXBoZXJ0ZXh0IjoiYmFzZTY0LWtleS1nb2VzLWhlcmUiLCJpdiI6IjRCSDlKREVmYU1RM3NHTGkiLCJ2ZXJzaW9uIjoiNC4wLjAiLCJoYW5kbGUiOiJ1dWlkLWdvZXMtaGVyZSJ9",
        "keyVersion": {
            "id": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6"
        }
    }
    ```
    {: screen}

    The `plaintext` value represents the unwrapped DEK, and the `ciphertext`
    value represents the wrapped DEK and are both base64 encoded. The
    `keyVersion.id` value represents the version of the root key that was used
    for wrapping.

    If you want {{site.data.keyword.keymanagementserviceshort}} to generate a
    new data encryption key (DEK) on your behalf, you can also pass in an empty
    body on a wrap request. Your generated DEK, containing the base64 encoded
    key material, is returned in the response entity-body, along with the
    wrapped DEK.
    {: tip}
