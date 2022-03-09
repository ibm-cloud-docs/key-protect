---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-09"

keywords: unwrap key, decrypt key, decrypt data encryption key

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
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Unwrapping keys
{: #unwrap-keys}

{{site.data.keyword.keymanagementservicelong}} combines the strength of multiple algorithms to protect the privacy and the integrity of your encrypted data. You can unwrap a [data encryption key](#x4791827){: term} (DEK), to access its contents by using the {{site.data.keyword.keymanagementservicefull}} API and Console. Unwrapping a DEK decrypts and checks the integrity of its contents, returning the original key material to your {{site.data.keyword.cloud_notm}} data service.
{: shortdesc}

To learn how key wrapping helps you control the security of at rest data in the
cloud, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
{: tip}

## Unwrapping a key using the console
{: #unwrapping-keys-ui}
{: ui}

If you already have an instance of {{site.data.keyword.keymanagementserviceshort}} and you wish to encrypt your DEK by using a graphical interface,
you can use the {{site.data.keyword.cloud_notm}} console.

After you import or create your [own keys](/docs/key-protect?topic=key-protect-create-root-keys),
complete the following steps to wrap your data using the key:

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login/){: external}.

1. From the **Menu**, choose the **Resource List** to view a list of your resources.

1. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

1. Choose the Root key that you used to originally wrap your data from the list of your keys.

1. Click the `⋯` icon to open a list of options for the DEK that you want to unwrap.

1. Click the **Envelope encryption** option to open the side panel. Choose the "Unwrap key" tab if it is not already highlighted.

1. Supply any required text for the above in the appropriate fields, including the ciphertext and any additional data you stored locally for the purpose of unwrapping the DEK.

1. Click **Unwrap Key** button.

## Unwrapping keys by using the API
{: #unwrap-key-api}
{: api}

[After you make a wrap call to the service](/docs/key-protect?topic=key-protect-wrap-keys),
you can unwrap a specified data encryption key (DEK) to access its contents by
making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/unwrap
```
{: codeblock}

Root keys that contain the same key material can unwrap the same data encryption
key (WDEK).
{: note}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Copy the ID of the root key that you used to perform the initial wrap
    request.

    You can find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Copy the `ciphertext` value that was returned during the initial wrap
    request.

4. Run the following `curl` command to decrypt and authenticate the key
    material.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/unwrap" \
        -H "accept: application/vnd.ibm.kms.key_action+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key_action+json" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "correlation-id: <correlation_ID>" \
        -d '{
                "ciphertext": "<encrypted_data_key>",
                "aad": [
                    "<additional_data>",
                    "<additional_data>"
                ]
            }'
    ```
    {: codeblock}

Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br> For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The unique identifier or alias for the root key that you used for the initial wrap request.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|additional_data|**Optional**.The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supply AAD when you make a wrap call to the service, you must specify he same AAD during the unwrap call.|
|encrypted_data_key|**Required**. The ciphertext value that was returned during a wrap operation.|
{: caption="Table 1. Describes the variables that are needed to unwrap keys in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

The original key material is returned in the response entity-body. The
response body also contains the ID of the key version that was used to
unwrap the supplied ciphertext.

The plaintext that is returned is base64 encoded. For more information on
how to decode your key material, see
[Decoding your key material](#how-to-decode-key-material).
The following JSON object shows an example returned value.

```json
{
    "plaintext": "Rm91ciBzY29yZSBhbmQgc2V2ZW4geWVhcnMgYWdv",
    "keyVersion": {
        "id": "02fd6835-6001-4482-a892-13bd2085f75d"
    }
}
```

If {{site.data.keyword.keymanagementserviceshort}} detects that you rotated
the root key that is used to unwrap and access your data, the service also
returns a newly wrapped data encryption key (`ciphertext`) in the unwrap
response body. The latest key version (`rewrappedKeyVersion`) that is
associated with the new `ciphertext` is also returned. Store and use the new
`ciphertext` value for future envelope encryption operations so that your
data is protected by the latest root key.

## Decoding your key material
{: #how-to-decode-key-material}

When you unwrap a data encryption key, the key material is returned in base64
encoding. You will need to decode the key before encrypting it.

### Using OpenSSL to decode key material
{: #open-ssl-encoding-root-unwrap}

1. Download and install
    [OpenSSL](https://github.com/openssl/openssl#for-production-use){: external}.

2. Decode your base64 encoded key material string by running the following
    command:

    ```sh
    openssl base64 -d -in <infile> -out <outfile>
    ```
    {: pre}

Replace the variables in the example request according to the following
table.

|Variable|Description|
|--- |--- |
|infile|The name of the file where your base64 encoded key material string resides.|
|outfile|The name of the file where your decoded key material will be be outputted once the command has ran.|
{: caption="Table 2. Describes the variables that are needed to decode your key material." caption-side="top"}

If you want to output the decoded material in the command line directly rather
than a file, run the command
`openssl enc -base64 -d <<< '<key_material_string>'`, where
key_material_string is the returned plaintext from your unwrap request.
{: note}


