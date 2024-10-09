---

copyright:
  years: 2017, 2024
lastupdated: "2024-10-09"

keywords: wrap key, encrypt data encryption key, envelope encryption API examples

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

# Wrapping keys
{: #wrap-keys}

You can manage and protect your encryption keys with a root key by using the
{{site.data.keyword.keymanagementservicelong}} API and Console.
{: shortdesc}

When you wrap a [data encryption key](#x4791827){: term} with a
[root key](#x6946961){: term} (DEK), {{site.data.keyword.keymanagementserviceshort}}
combines the strength of multiple algorithms to protect the privacy and the
integrity of your encrypted data.

To learn how key wrapping helps you control the security of at rest data in the
cloud, see
[Protecting data with envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
{: tip}

## Wrapping a key using the console
{: #wrapping-keys-ui}
{: ui}

If you already have an instance of {{site.data.keyword.keymanagementserviceshort}} and wish to encrypt your DEK by using a graphical interface, you can use the {{site.data.keyword.cloud_notm}} console.

After you import or create your [own keys](/docs/key-protect?topic=key-protect-create-root-keys),
complete the following steps to wrap your data using the key:

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. From the **Menu**, choose the **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Choose the Root key from the list of your keys that you want to use to wrap your data.

5. Click the `⋯` icon and then click the **Envelope encryption** option to open the side panel. Make sure the "Wrap key" option is selected.

7. You have a choice of whether to allow {{site.data.keyword.keymanagementserviceshort}} to wrap your data or whether you want to provide your own base64 encoded plaintext and additional authentication data (AAD), as needed, to wrap the data. Note that if you choose to provide your own plaintext and AAD, you will have to provide it when unwrapping your data. If you do not have experience creating AAD and base 64 encoded plaintext, the recommended practice is to allow {{site.data.keyword.keymanagementserviceshort}} to wrap your data by leaving the **Wrap key for me** option selected.

8. Click **Wrap Key** button.

## Wrapping keys by using the API
{: #wrap-key-api}
{: api}

You can protect a specified data encryption key (DEK) with a root key that you
manage in {{site.data.keyword.keymanagementserviceshort}}.

[After you designate a root key in the service](/docs/key-protect?topic=key-protect-create-root-keys),
you can wrap a DEK with advanced encryption by making a `POST` call to the
following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/wrap
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service.](/docs/key-protect?topic=key-protect-set-up-api)

2. Copy the key material of the DEK that you want to manage and protect.

    If you have manager or writer privileges for your
    {{site.data.keyword.keymanagementserviceshort}} instance,
    [you can retrieve the key material for a specific key by making a `GET /v2/keys/<keyID_or_alias>` request](/docs/key-protect?topic=key-protect-view-keys#view-keys-api).

3. Copy the ID of the root key that you want to use for wrapping.

4. Run the following `curl` command to protect the key with a wrap operation.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/wrap" \
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

Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The unique identifier or alias for the root key that you want to use for wrapping.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request. Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default. or more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|data_key|**Optional**.The key material of the DEK that you want to manage and protect. The plaintext value must be base64 encoded. For more information on encoding your key material, see [Encoding your key material](/docs/key-protect?topic=key-protect-import-root-keys#how-to-encode-root-key-material). To generate a new DEK, omit the plaintext attribute. The service generates a random plaintext (32 bytes), wraps that value, and then returns both the generated and wrapped values in the response. The generated and wrapped values are base64 encoded and you will need to decode them in order to decrypt the keys.|
|additional_data|**Optional**.The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supply AAD when you make a wrap call to the service, you must specify the same AAD during the subsequent unwrap call.Important: The {{site.data.keyword.keymanagementserviceshort}} service does not save additional authentication data. If you supply AAD, save the data to a secure location to ensure that you can access and provide the same AAD during subsequent unwrap requests.|
{: caption="Describes the variables that are needed to wrap a specified key in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}

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
