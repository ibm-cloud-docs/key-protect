---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-16"

keywords: automatic key rotation, set rotation policy, retrieve key policy

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

# Setting a rotation policy
{: #set-rotation-policy}

You can set an automatic rotation policy for a root key by using
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

When you set an automatic rotation policy for a root key, you shorten the
lifetime of the key at regular intervals, and you limit the amount of
information that is protected by that key.

You can create a rotation policy only for root keys that are generated in
{{site.data.keyword.keymanagementserviceshort}}. If you imported the root key
initially, you must provide new base64 encoded key material to rotate the key.
For more information, see
[Rotating root keys on-demand](/docs/key-protect?topic=key-protect-rotate-keys).
{: note}

Want to learn more about your key rotation options in
{{site.data.keyword.keymanagementserviceshort}}?
For more information, see
[Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: tip}

## Managing rotation polices in the console
{: #manage-policies-gui}
{: ui}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** > **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in your service. If you have many keys, you can narrow your search by using the search bars to only search for enabled keys (since other kinds of keys cannot be rotated), keys in a particular key ring, and keys with a particular alias.

5. Once the key has been found, click the ⋯ icon to open a list of options for the key that you want to rotate.

6. From the options menu, click **Rotate** to open the **Rotation** side panel.

7. Under **Manage rotation policy**, select the 30-day intervals for key rotation you want. If a key is set to be rotated every `2` months, for example, it will be rotated every 60 days, regardless of the number of days in a particular month.

8. Click **Set policy** to establish this policy. If you want to rotate the key immediately, click **Rotate key**. Note: these actions are not mutually exclusive. If your key has an existing rotation policy, the interface displays the key's existing rotation period.

**For imported root keys only**, you must add base64 encoded key material that you want to store and manage in the service. Ensure that the key material is in 128, 192, or 256 bits and that the bytes of data (for example, 32 bytes for 256 bits) are encoded by using base64 encoding.
{: important}

When it's time to rotate the key based on the rotation interval that you specify, {{site.data.keyword.keymanagementserviceshort}} automatically replaces the root key with new key material.

## Managing rotation policies with the API
{: #manage-rotation-policies-api}
{: api}

### Viewing a rotation policy
{: #view-rotation-policy-api}
{: api}

For a high-level view, you can browse the rotation policies that are associated
with a root key by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the rotation policy for a specified key by running the following
    `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|key_ID|**Required**. The unique identifier for the root key that has an existing rotation policy.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|The unique identifier that is used to track and correlate transactions.|
{: caption="Table 1. Describes the variables that are needed to retrieve a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `GET api/v2/keys/{id}/policies` response returns policy details
that are associated with your key. The following JSON object shows an
example response for a root key that has an existing rotation policy.

```json
{
    "metadata": {
        "collectionTotal": 1,
        "collectionType": "application/vnd.ibm.kms.policy+json"
    },
    "resources": [
        {
            "id": "02fd6835-6001-4482-a892-13bd2085f75d",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "...",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "...",
            "updatedat": "2019-03-06T16:31:05Z"
        }
    ]
}
```
{: screen}

The `interval_month` value indicates the key rotation frequency in months.

### Creating a rotation policy
{: #create-rotation-policy-api}
{: api}

Create a rotation policy for your root key by making a `PUT` call to the
following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a rotation policy for a specified key by running the following `curl`
    command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -H "correlation-id: <correlation_ID>" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.policy+json",
                        "rotation": {
                            "interval_month": <rotation_interval>
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|key_ID|**Required**. The unique identifier for the root key that you want to create a rotation policy for.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**The unique identifier that is used to track and correlate transactions.|
|rotation_interval|**Required**. An integer value that determines the key rotation interval time in months. The minimum is 1 and the maximum is 12.|
{: caption="Table 2. Describes the variables that are needed to create a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `PUT api/v2/keys/{id}/policies` response returns policy details
that are associated with your key. The following JSON object shows an
example response for a root key that has an existing rotation policy.

```json
{
    "metadata": {
        "collectionTotal": 1,
        "collectionType": "application/vnd.ibm.kms.policy+json"
    },
    "resources": [
        {
            "id": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "...",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "...",
            "updatedat": "2019-03-06T16:31:05Z"
        }
    ]
}
```
{: screen}

### Updating a rotation policy
{: #update-rotation-policy-api}
{: api}

Update an existing policy for a root key by making a `PUT` call to the following
endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Replace the rotation policy for a specified key by running the following
    `curl` command.

    ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -H "correlation-id: <correlation_ID>" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.policy+json",
                        "rotation": {
                            "interval_month": <new_rotation_interval>
                        }
                    }
                ]
            }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|key_ID|**Required**. The unique identifier for the root key that you want to replace a rotation policy for.|
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|new_rotation_interval|**Required**. An integer value that determines the key rotation interval time in months. The minimum is 1 and the maximum is 12.|
{: caption="Table 3. Describes the variables that are needed to update a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `PUT api/v2/keys/{id}/policies` response returns updated policy
details that are associated with your key. The following JSON object shows
an example response for a root key with an updated rotation policy.

```json
{
    "metadata": {
        "collectionTotal": 1,
        "collectionType": "application/vnd.ibm.kms.policy+json"
    },
    "resources": [
        {
            "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "...",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "...",
            "updatedat": "2019-03-10T12:24:22Z"
        }
    ]
}
```
{: screen}

The `interval_month` and `updatedat` values are updated in the policy
details for the key. If a different user updates a policy for a key that you
created initially, the `updatedby` value also changes to show the identifier
for the person who sent the request.


