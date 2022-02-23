---

copyright:
  years: 2019, 2020
lastupdated: "2020-11-24"

keywords: enable BYOK, onboard to Key Protect, Key Protect onboarding, internal, KYOK, BYOK

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

# Set up the {{site.data.keyword.keymanagementserviceshort}} API
{: #configure-api}

Create a test instance and add code to use our wrap and unwrap APIs.
{: shortdesc}

## Before you begin
{: #configure-prereqs}

- You must have {{site.data.keyword.iamshort}} (IAM) enabled for your service.
For questions and help with adoption, see the
[IAM onboarding guide](/docs/get-coding?topic=get-coding-getting-started-iam-onboarding-overview){: external}
or the
[`#iam-adopters` Slack channel](https://ibm-cloudplatform.slack.com/archives/iam-adopters){: external}.
- You must have a basic understanding of IAM key concepts, such as
[granting service to service access](/docs/get-coding?topic=get-coding-servicetoservice){: external}.
- You must have a basic understanding of the
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption)
process. See
[Integrations](/docs/key-protect?topic=key-protect-integrate-services)
to learn how {{site.data.keyword.keymanagementserviceshort}} integrates with
other cloud data services.

## Step 1. Create a test instance
{: #configure-provision-service}

First, create a test {{site.data.keyword.keymanagementserviceshort}} instance so
that you can set up the APIs. Normally, your customer is provisioning their own
instance of a KMS-enabled service.

1. [Provision an instance of the service](/docs/key-protect?topic=key-protect-provision)

2. [Retrieve an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID)

## Step 2. Create a root key
{: #create-root}

Next, create a
[root key](/docs/key-protect?topic=key-protect-envelope-encryption#key-types)
in your test instance. Root keys cover two main use cases:

- To protect encrypted data at rest with
[envelope encryption](/docs/key-protect?topic=key-protect-envelope-encryption).
This process is done by using a root key to wrap and unwrap the data encryption
keys (DEKS) that are stored in your service.

- To enable Bring Your Own Key (BYOK) capabilities for your service. Also called
_customer-managed encryption_, this model enables support for user-provided root
keys that the customer can manage in
{{site.data.keyword.keymanagementserviceshort}}.

For more information, see
[Adoption Patterns](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/architecture/adoption/){: external}.
{: tip}

You can programmatically create a root key in
{{site.data.keyword.keymanagementserviceshort}} by running the following
command.

### Example request
{: #example-create}

```sh
$ curl -X POST \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "content-type: application/vnd.ibm.kms.key+json" \
    -H "correlation-id: <correlation_ID>" \
    -d '{
            "metadata": {
                "collectionType": "application/vnd.ibm.kms.key+json",
                "collectionTotal": 1
            },
            "resources": [
                {
                    "type": "application/vnd.ibm.kms.key+json",
                    "name": "<key_alias>",
                    "description": "<key_description>",
                    "extractable": false
                }
            ]
        }'
```
{: codeblock}

Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your service to service access token. Include the full contents of the token, including the Bearer value, in the curl request.|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**. The unique identifier that is used to track and correlate transactions.|
|key_alias|**Required**. A unique, human-readable name for easy identification of your key.|
|key_description|**Optional**. An extended description of your key.|
{: caption="Table 1. Describes the variables needed to create a root key." caption-side="top"}

Verify that the key was created by running the following call to browse the keys
in your {{site.data.keyword.keymanagementserviceshort}} instance.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "correlation-id: <correlation_ID>"
```
{: codeblock}

A successful response returns the ID value for the key, along with other
metadata. The ID is a unique identifier that is assigned to the key and is used
for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

Keep in mind that it is the customer's responsibility to add and centrally
manage root keys in their {{site.data.keyword.keymanagementserviceshort}}
instance. As a integrated service, you can retrieve the key ID or key
CRN values from your customer's instance by
[listing the keys that are available in the specified instance](/apidocs/key-protect#list-keys){: external}.
{: note}

## Step 3. Wrap a data encryption key
{: #wrap-key}

You can use the returned root key ID to wrap a data encryption key (DEK). For
each service that adopts {{site.data.keyword.keymanagementserviceshort}}, the
DEK will be different. For example, the DEK might be a LUKS passphrase or an
actual AES key. To protect the DEK with the root key, use the wrap API.

Note: As an alternative, we can also generate the DEK for you.

### Example request
{: #example-wrap}

```sh
$ curl -X POST \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/wrap" \
    -H "accept: application/vnd.ibm.kms.key_action+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "content-type: application/vnd.ibm.kms.key_action+json" \
    -H "correlation-id: <correlation_ID>" \
    -H "prefer: return=minimal" \
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
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to use for wrapping.|
|IAM_token|**Required**. Your service to service access token. Include the full contents of the token, including the Bearer value, in the curl request.|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|data_key|**Optional**. The key material of the DEK that you want to manage and protect. The plaintext value must be base64 encoded. To generate a new DEK, omit the plaintext attribute. The service generates a random plaintext (32 bytes) and wraps that value. In the response, plaintext contains the unwrapped DEK and ciphertext contains the wrapped value.|
|additional_data|**Optional**. The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters. If you supply AAD when you make a wrap call to the service, you must specify the same AAD during the subsequent unwrap call.|
{: caption="Table 2. Describes the variables needed to wrap a specified key in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}


The wrapped data encryption key (wDEK), containing the base64 encoded key
material, is returned in the response entity-body. The following JSON object
shows an example returned value.

### Note
{: #note-wdek}

wDEKs are not stored in the {{site.data.keyword.keymanagementserviceshort}}
service.

```json
{
    "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
    "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9",
    "aad": [
        "data1",
        "data2"
    ]
}
```
{: screen}

## Step 4. Unwrap a data encryption key
{: #unwrap-key}

To unwrap a data encryption key (DEK), provide the wDEK (the `ciphertext` value)
in your unwrap request to {{site.data.keyword.keymanagementserviceshort}}. To
ensure that your unwrap request succeeds, provide the same root key that you
used during the initial wrap request.

### Example request
{: #example-unwrap}

```sh
$ curl -X POST \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/unwrap" \
    -H "accept: application/vnd.ibm.kms.key_action+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>" \
    -H "content-type: application/vnd.ibm.kms.key_action+json" \
    -H "correlation-id: <correlation_ID>" \
    -H "prefer: return=minimal" \
    -d '{
            "ciphertext": "<wrapped_data_key>",
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
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you used for wrapping.|
|IAM_token|**Required**. Your service to service access token. Include the full contents of the token, including the Bearer value, in the curl request.|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**. The unique identifier that is used to track and correlate transactions.|
|additional_data|**Optional**. The additional authentication data (AAD) that is used to further secure the key. Each string can hold up to 255 characters.<br><br>Important: If you supply AAD when you make a wrap call to the service, you must specify the same AAD during the unwrap call.|
|wrapped_data_key|**Required**. The ciphertext value returned during a wrap operation.|
{: caption="Table 3. Describes the variables needed to unwrap keys in {{site.data.keyword.keymanagementserviceshort}}." caption-side="top"}


The original base64 encoded key material is returned in the response
entity-body. The following JSON object shows an example returned value.

```json
{
    "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
}
```
{: screen}

## Next steps
{: #next-steps}

Success! Your service now supports envelope encryption, which is one of the core
capabilities needed. Now, you can map your protected resources to the root key.

- Head over to start
[key registration](/docs/key-protect?topic=key-protect-register-protected-resources)


