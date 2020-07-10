---

copyright:
  years: 2017, 2020
lastupdated: "2020-07-10"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# Unwrapping keys
{: #unwrap-keys}

You can unwrap a data encryption key to access its contents by using the
{{site.data.keyword.keymanagementservicefull}} API. Unwrapping a DEK decrypts
and checks the integrity of its contents, returning the original key material to
your {{site.data.keyword.cloud_notm}} data service.
{: shortdesc}

To learn how key wrapping helps you control the security of at-rest data in the
cloud, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
{: tip}

## Unwrapping keys by using the API
{: #unwrap-key-api}

[After you make a wrap call to the service](/docs/key-protect?topic=key-protect-wrap-keys),
you can unwrap a specified data encryption key (DEK) to access its contents by
making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

Root keys that contain the same key material can unwrap the same data encryption key (WDEK).
{: note}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Copy the ID of the root key that you used to perform the initial wrap request.

  You can find the ID for a key in your service instance by
  [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
  or by accessing the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Copy the `ciphertext` value that was returned during the initial wrap
request.

4. Run the following cURL command to decrypt and authenticate the key material.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
        "ciphertext": "<encrypted_data_key>",
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
          <strong>Required.</strong> The unique identifier for the root key that
          you used for the initial wrap request.
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
          <varname>correlation_ID</varname>
        </td>
        <td>
          The unique identifier that is used to track and correlate
          transactions.
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
          the same AAD during the unwrap call.
        </td>
      </tr>

      <tr>
        <td>
          <varname>encrypted_data_key</varname>
        </td>
        <td>
          <strong>Required.</strong> The <code>ciphertext</code> value that was
          returned during a wrap operation.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to unwrap keys in
        {{site.data.keyword.keymanagementserviceshort}}.
      </caption>
    </table>

    The original key material is returned in the response entity-body. The
    response body also contains the ID of the key version that was used to
    unwrap the supplied ciphertext. 

    The plaintext that is returned is base64 encoded. For more information on how to decode your key material, see 
    [Decoding your key material](#how-to-decode-key-material).
    {: important}
    

    The following JSON object shows an example returned value.

    ```json
    {
      "plaintext": "Rm91ciBzY29yZSBhbmQgc2V2ZW4geWVhcnMgYWdv",
      "keyVersion": {
        "id": "02fd6835-6001-4482-a892-13bd2085f75d"
      }
    }
    ```
    {: screen}


    If {{site.data.keyword.keymanagementserviceshort}} detects that you rotated
    the root key that is used to unwrap and access your data, the service also
    returns a newly wrapped data encryption key (`ciphertext`) in the unwrap
    response body. The latest key version (`rewrappedKeyVersion`) that is
    associated with the new `ciphertext` is also returned. Store and use the new
    `ciphertext` value for future envelope encryption operations so that your
    data is protected by the latest root key.
    

## Decoding your key material
{: #how-to-decode-key-material}

When you unwrap a data encryption key, the key material is returned in base64 encoding. You will need to decode the key before encrypting it.

### Using OpenSSL to dencrypt key material
{: #open-ssl-encoding-root}

1. Download and install [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.
2. Base64 encode your key material string by running the following command:

    ```
    $ openssl base64 -d -in <infile> -out <outfile>
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
          <varname>infile</varname>
        </td>
        <td>
          <p>
            The name of the file where your base64 encoded key material string resides.
          </p>
        </td>
      </tr>
      <tr>
        <td>
          <varname>outfile</varname>
        </td>
        <td>
          <p>
            The name of the file where your decoded key material will be be outputted once the command has ran.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 3. Describes the variables that are needed to decode your key material.
      </caption>
    </table>

  If you want to output the decoded material in the command line directly rather
  than a file, run the command `openssl enc -base64 -d <<< '<key_material_string>'`,
  where key_material_string is the returned plaintext from your unwrap request.
  {: note}
