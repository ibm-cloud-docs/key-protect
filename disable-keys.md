---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-12"

keywords: disable key, enable key, suspend operations on a key

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
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Disabling and enabling root keys
{: #disable-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to disable or enable
a root key and temporarily revoke access to the key's associated data on the
cloud.
{: shortdesc}

As an admin, you might need to temporarily disable a root key if you suspect a
possible security exposure, compromise, or breach with your data. When you
disable a root key, you suspend its encrypt and decrypt operations. After
confirming that a security risk is no longer active, you can restore access to
your data by enabling the disabled root key.

If you are using a Cloud Service that is integrated with
{{site.data.keyword.keymanagementserviceshort}}, your data might not be
accessible after disabling a root key. To determine whether an
[integrated service](/docs/key-protect?topic=key-protect-integrate-services)
supports revoking access to data by disabling a
{{site.data.keyword.keymanagementserviceshort}} root key, refer to its service
documentation.
{: note}

## Disabling or enabling a root key
{: #disable-enable-root-key}

### Disabling a root key
{: #disable-root-key}

When you disable a root key that was previously enabled, the key transitions
from the
[_Active_](/docs/key-protect?topic=key-protect-key-states)
to the
[_Suspended_](/docs/key-protect?topic=key-protect-key-states)
key state. This action mean the key can no longer be used to cryptographically
protect data.

If you're using an integrated Cloud Service that supports revoking access to a
disabled root key, the service may take up to a maximum of 4 hours before access
to the root key's associated data is revoked.

After access to the associated data is revoked, a corresponding `disable event`
is displayed in the {{site.data.keyword.at_full}} web UI. The `disable event` indicates the
key has been revoked (and is now disabled) and the key can **not** be used for
encrypt and decrypt operations.
{: note}

### Enabling a root key
{: #enable-root-key}

When you enable a root key that was previously disabled, the key transitions
from the
[_Suspended_](/docs/key-protect?topic=key-protect-key-states)
to the
[_Active_](/docs/key-protect?topic=key-protect-key-states)
key state. This action restores the key's encrypt and decrypt operations.

If you're using an integrated Cloud Service that supports restoring access to a
disabled root key, the service may take up to a maximum of 4 hours before access
to the root key's associated data is restored.

After access to the associated data is restored, a corresponding `enable event`
is displayed in the {{site.data.keyword.at_full_notm}} web UI. The `enable event` indicates the
key has been restored (and is now enabled) and the key can be used for encrypt
and decrypt operations.
{: note}

## Disabling and enabling root keys in the console
{: #disable-enable-ui}
{: ui}

If you prefer to enable or disable your root keys by using a graphical
interface, you can use the IBM Cloud console.

### Disabling a root key in the console
{: #disable-ui}
{: ui}

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys),
complete the following steps to disable a key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in
   your {{site.data.keyword.keymanagementserviceshort}} instance.

5. Click the ⋯ icon to open a list of options for the key that you want to
   disable.

6. From the options menu, click **Disable key** and confirm the key was disabled
   in the updated **Keys** table.

### Enabling a root key in the console
{: #enable-ui}
{: ui}

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys) and [disable](#disable-ui) a root key,
complete the following steps to enable the key:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in
   your service.

5. Click the ⋯ icon to open a list of options for the key that you want to
   enable.

6. From the options menu, click **Enable key** and confirm the key was enabled in the updated **Keys** table.

Keys cannot be enabled immediately after being disabled. If a key was disabled in error, wait at least 30 seconds before attempting to re-enable it.
{: tip}

## Disabling and enabling root keys with the API
{: #disable-enable-api}
{: api}

### Disabling a root key with the API
{: #disable-api}
{: api}

You can disable a root key that's in the _Active_ key state by making a `POST`
call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/disable
```

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api#retrieve-credentials).

    To disable a root key, you must be assigned a _Manager_ service access role
    for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the ID of the root key that you want to disable.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    [request](/apidocs/key-protect#list-keys){: external},
    or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to disable the root key and suspend its
   encrypt and decrypt operations.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/disable" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|****Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to disable.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 1. Describes the variables that are needed to disable root keys with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful disable request returns an HTTP `204 No Content` response,
which indicates that the root key was disabled for encrypt and decrypt
operations.

### Optional: Verify key disablement
{: #disable-api-verify}
{: api}

You can verify that a key has been disabled by issuing a get key metadata request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<key_id>` is the ID of the key, the `<instance_ID>` is the name of your instance, and your `<IAM_token>` is your IAM token.

Review the `state` field in the response body to verify that the key
transitioned to the _Suspended_ key state. The following JSON output shows
the metadata details for a disabled root key.

The integer mapping for the _Suspended_ key state is 2. Key States are based
on NIST SP 800-57.
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
            "state": 2,
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

### Enabling a disabled root key with the API
{: #enable-api}
{: api}

You can enable a root key that's in the _Suspended_ key state by making a `POST`
call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/enable
```

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To enable a root key, you must be assigned a _Manager_ service access role
    for the instance or key. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Retrieve the ID of the disabled root key that you want to enable.

    You can retrieve the ID for a specified key by making a `GET /v2/keys`
    [request](/apidocs/key-protect#list-keys){: external},
    or by viewing your keys in the
    {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Run the following `curl` command to enable the root key and restore its
   encrypt and decrypt operations.

    You must wait 30 seconds after disabling a root key before you are able to
    enable it again.
    {: note}

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/enable" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>"
    ```
    {: codeblock}

Replace the variables in the example request according to the following
table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The unique identifier for the root key that you want to enable.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the key ring that the key is a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is recommended to specify the key ring ID for a more optimized request.<br><br>Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: default.<br><br>For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
{: caption="Table 2. Describes the variables that are needed to enable root keys  with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful enable request returns an HTTP `204 No Content` response, which
indicates that the root key was reinstated for encrypt and decrypt
operations.

### Optional: Verify key enablement
{: #enable-key-api-verify}
{: api}

You can verify that a key has been enabled by issuing a get key metadata request:

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_id>/metadata" \
    -H "accept: application/vnd.ibm.kms.key+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Where the `<key_id>` is the ID of the key, the `<instance_ID>` is the name of your instance, and your `<IAM_token>` is your IAM token.

Review the `state` field in the response body to verify that the root key
transitioned to the _Active_ key state. The following JSON output shows the
metadata details for
an active key.

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
