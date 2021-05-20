---

copyright:
  years: 2017, 2020, 2021
lastupdated: "2021-01-04"

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

Reencrypt your data encryption keys by using the
{{site.data.keyword.keymanagementservicelong}} API.
{: shortdesc}

When you
[rotate a root key in {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-key-rotation),
new cryptographic key material becomes available for protecting the data
encryption keys (DEKs) that are associated with the root key. With the rewrap
API, you can reencrypt or rewrap your DEKs without exposing the keys in their
plaintext form.

To learn how envelope encryption helps you control the security of at rest data
in the cloud, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).

## Rewrapping keys by using the API
{: #rewrap-key-api}

You can reencrypt a specified data encryption key (DEK) with a root key that you
manage in {{site.data.keyword.keymanagementserviceshort}}, without exposing the
DEK in its plaintext form.

Rewrapping keys works by combining `unwrap` and `wrap` calls to the service. For
example, you can emulate a `rewrap` operation by first calling the `unwrap` API
to access a DEK, and then calling the `wrap` API to reencrypt the DEK by using
the newest root key material.
{: note}

[After you rotate a root key in the service](/docs/key-protect?topic=key-protect-rotate-keys),
rewrap a data encryption key that is associated with the root key by making a
`POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rewrap
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Copy the ID of the rotated root key that you used to perform the initial wrap
   request.

    You can retrieve the ID for a key by making a `GET api/v2/keys` request, or
    by viewing your keys in the {{site.data.keyword.keymanagementserviceshort}}
    GUI.

3. Copy the `ciphertext` value that was returned during the latest wrap request.

4. Rewrap the key with the latest root key material by running the following
   `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/rewrap" \
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

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you used for the initial wrap request.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|Optional. The unique identifier of the key ring that the key belongs to. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|The unique identifier that is used to track and correlate transactions.|
|encrypted_data_key|**Required**. The ciphertext value that was returned by the original wrap operation.|
|additional_data|**Optional**The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supplied AAD for the initial wrap call, you must specify the same AAD during the subsequent unwrap or rewrap calls.<br><br>Important: The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap or rewrap requests.|
{: caption="Table 1. Describes the variables that are needed to rewrap keys in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

The newly wrapped data encryption key, original key version (`keyVersion`)
that is associated with the supplied ciphertext and latest key version
(`rewrappedKeyVersion`) associated with the new ciphertext is returned in
the response entity-body. The following JSON object shows an example
returned value.

```json
{
    "ciphertext": "eyJjaX ... h0Ijoi ... c1ZCJ9",
    "keyVersion": {
        "id": "02fd6835-6001-4482-a892-13bd2085f75d"
    },
    "rewrappedKeyVersion": {
        "id": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6"
    }
}
```
{: screen}

Store and use the new `ciphertext` value for future envelope encryption
operations so that your data is protected by the latest root key.



5. Optional. Verify that the key was successfully rewrapped by base64 decoding
   the `ciphertext` value.

    ```sh
    $ echo <ciphertext> | base64 --decode ; echo
    ```
    {: codeblock}

    Replace `<ciphertext>` with the base64 encoded value that was returned in
    the previous step. The following JSON object shows an example CLI output.

    ```json
    {
      "ciphertext": "mIzRrwZAA8+WqRckG6gt1ji8HlEEJPSiV+TRBSR4GVr+FlAZlC5KvRriRF0=",
      "iv": "lbwxXlAW2DS7+5jGz5Y1Kg==",
      "version": "4.0.0",
      "handle": "8e309bae-b3ec-4270-9b87-89f8697fe54f"
    }
    ```
    {: screen}

    QUESTION: How do I know that the wDEK has been rewrapped? Does the version
    number change, or just the ciphertext value? What do the iv, version, and
    handle values represent (internal security parameters?)


