---

copyright:
  years: 2020, 2024
lastupdated: "2024-04-18"

keywords: key rings, group keys, manage key groups

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

# Grouping keys together using key rings
{: #grouping-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to create
a group of keys for a target group of users that require the same IAM
access permissions.
{: shortdesc}

As an account admin, you can bundle the keys in your
{{site.data.keyword.keymanagementserviceshort}} service instance into groups
called "key rings". A key ring is a collection of keys, within your
service instance, that all require the same IAM access permissions.
For example, if you have a group of team members who will need a particular
type of access to a specific group of keys, you can create a key ring for those
keys and assign the appropriate IAM access policy to the target user group. The
users that are assigned access to the key ring can create and manage the resources
that exist within the key ring.

Key rings are also useful in cases where it is important for one business unit to
have access to a set of keys that another business unit should not have. An account admin
can create key rings for each business unit and [assign](/docs/key-protect?topic=key-protect-grouping-keys&interface=ui#grant-access-key-ring) the appropriate
level of access to the appropriate users. In the case that the account admin would like to delegate
platform management of a specific key ring to someone else, they can assign a user a
[platform administrator role at the key ring level](/docs/account?topic=account-userroles#platformroles).
The sub administrator will then have the ability to manage the key ring and grant access to the appropriate users.

You can grant access to key rings within a
{{site.data.keyword.keymanagementserviceshort}} instance by using the
{{site.data.keyword.cloud_notm}} console, IAM API, or IAM CLI.
{: note}

Before you create a key ring for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **Every {{site.data.keyword.keymanagementserviceshort}} instance comes with a default key ring.**
    Each newly created {{site.data.keyword.keymanagementserviceshort}} instance comes with
    a generated key ring with an ID of `default`. All keys that are not associated
    with an otherwise specified key ring exists within the default key ring.

- **Key rings can hold standard and root keys.**
    Key rings can contain both standard and root keys. There is not a limit on how
    many keys can exist within a key ring.

- **A key can only be a part of one key ring at a time.**
    A key can only be a part of one key ring. Key ring assignment happens upon key creation. If a key ring id is not passed in upon creation, the key will be a part of the `default` key ring.

The maximum amount of key rings is 50 per service instance.
{: note}

## Creating key rings with the UI
{: #create-key-ring-ui}
{: ui}

You must have either the service "Writer" or "Manager" role to create a key ring.
{: important}

To create a key ring:

1. Click on **Key rings** in the left navigation.
2. In the **Key rings** panel, click the **Create** button.
3. In the **Create a key ring** tab, give your new key ring a name, following the instructions on permitted characters. Then click **Create**.

After it is created, your new key ring will appear in the list of key rings and you are able to transfer keys to it or create keys for it.

## Creating key rings with the API
{: #create-key-ring-api}
{: api}

Create a key ring by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a key ring by running the following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_id>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ring_id|**Required**. The unique identifier for the key ring that you would like to create.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
{: caption="Table 1. Describes the variables that are needed to create a key ring with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `POST api/v2/key_rings` request returns an HTTP `201 Created`
response, which indicates that the key ring was created and is now
available for to hold standard and root keys.

## Transferring a key to a different key ring
{: #transfer-key-key-ring}

As requirements change and new team members are brought into an org, you might create new key rings to reflect these organizational changes. After creating the key rings, it might be necessary to move a key from an existing key ring to a new key ring that has different IAM permissions. For example, you might be onboarding a team that will need specific access to a key that is part of to a custom, non-default key ring that was previously made. You can create a new key ring that is dedicated to the onboarding team and, since keys can only be associated with one key ring at a time, you will need to move the key to the new key ring.

After transferring a key to a different key ring, it may take up to a maximum of ten minutes for the change to take effect.
{: important}

### Transferring a key to a different key ring with the UI
{: #transfer-key-key-ring-ui}
{: ui}

If you do not see all of the options you expect to see, it might be because you do not have the permission to execute a particular action. Make sure your roles and permissions are sufficient to perform the action. For more information on roles, check out [Managing user access](/docs/key-protect?topic=key-protect-manage-access).

You must have the service "Manager" role of both the key being transferred and the target key ring to transfer a key.
{: important}

From the **Keys** panel:

1. Find the key you want to transfer. Note that it can be helpful to specify the key ring where the key is currently located by selecting the ring in the **Key ring ID** drop down list. You can also click on **Key rings** in the left navigation, find the appropriate key ring, and click **View associated keys** inside the setting drop-down list. This will show you all of the keys associated with that key ring.
2. Click on the ⋯ button, and select **Edit key ring** from the drop-down list.
3. In the drop-down list, select the key ring you want to move the key to. Then click **Save**.

### Transferring a key to a different key ring with the API
{: #transfer-key-key-ring-api}
{: api}

Transfer a key to a different key ring by making a `PATCH` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To update the key ring of a key, you must have at least _Manager_ service access to the key and the target key ring. To learn how IAM roles map to
    {{site.data.keyword.keymanagementserviceshort}} service actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Update the key ring of a key by running the following `curl` command.

    ```sh
    $ curl -X PATCH \
        https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias> \
        -H 'accept: application/vnd.ibm.kms.key+json' \
        -H 'authorization: Bearer <IAM_token>' \
        -H 'bluemix-instance: <instance_ID>' \
        -H 'content-type: application/vnd.ibm.kms.key+json' \
        -H "x-kms-key-ring: <original_key_ring_ID>" \
        -H "correlation-id: <correlation_ID>" \
        -d '{
        "keyRingID": "<new_key_ring_ID>"
        }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your{{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The unique identifier or alias for the key that you would like to update.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|original_key_ring_ID|**Optional**. The unique identifier of the key ring that the key is currently a part of. If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key in every key ring associated with the specified instance. It is therefore recommended to specify the key ring ID for a more optimized request. Note: The key ring ID of keys that are created without an `x-kms-key-ring` header is: `default`.|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
|new_key_ring_ID|**Required**. The unique identifier for the target key ring that you would like to move the key to.|
{: caption="Table 2. Describes the variables that are needed to update a key's key ring with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `PATCH api/v2/keys/keyID_or_alias` request returns the key's metadata, including the id of the key ring that the key is a part of.

``` json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "type": "application/vnd.ibm.kms.key+json",
            "id": "02fd6835-6001-4482-a892-13bd2085f75d",
            "name": "test-root-key",
            "aliases": [
                "alias-1",
                "alias-2"
            ],
            "description": "A test root key",
            "state": 1,
            "extractable": false,
            "keyRingID": "new-key-ring",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "imported": false,
            "creationDate": "2020-03-12T03:37:32Z",
            "createdBy": "...",
            "algorithmType": "Deprecated",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "Deprecated"
            },
            "algorithmBitSize": 256,
            "algorithmMode": "Deprecated",
            "lastUpdateDate": "2020-03-12T03:37:32Z",
            "keyVersion": {
                "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
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

## Granting access to a key ring
{: #grant-access-key-ring}
{: ui}

You can grant access to a key ring within a {{site.data.keyword.keymanagementserviceshort}} instance by using the {{site.data.keyword.cloud_notm}} console,  [IAM API](/apidocs/iam-policy-management#create-policy){: external}, or [CLI](/docs/cli?topic=cli-ibmcloud_commands_iam#ibmcloud_iam_user_policy_create){ :external}.

Review [roles and permissions](/docs/key-protect?topic=key-protect-manage-access) to learn how {{site.data.keyword.cloud_notm}} IAM roles map to {{site.data.keyword.keymanagementserviceshort}} actions.
{: tip}

To assign access to a key ring with the console:

1. From the menu bar, click **Manage** &gt; **Access (IAM)**, and select
    **Users** to browse the existing users in your account.

2. Select a table row, and click the ⋯ icon to open a list of options for that
    user.

3. From the options menu, click **Assign access**.

4. Click **Assign users additional access**.

5. Click the **IAM services** button.

6. From the list of services, select
    **{{site.data.keyword.keymanagementserviceshort}}**.

7. Select **Services based on attributes**.

8. Select the **Instance ID** attribute and select the instance in which the key
    ring resides.

9. Select the **Key Ring ID** attribute and enter the ID associated with the key ring.

8. Choose a combination of
    [platform and service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles)
    to assign access for the user.

9. Click **Add**.

10. Continue to add platform and service access roles as needed and when you are
    finished, click **Assign**. Note that the user must be assigned at least _Reader_
    access to the entire instance in order for them to list, create and delete key rings
    within the instance.

![The image shows an example of how to grant user access to a key ring.](images/key-ring-iam-policy.png){: caption="Figure 1. Shows how to grant user access to an instance." caption-side="bottom"}

## Listing key rings with the API
{: #list-key-ring-api}
{: api}

For a high-level view, you can browse the key rings that are managed in your
provisioned instance of {{site.data.keyword.keymanagementserviceshort}} by
making a GET call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys_rings
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. View general characteristics about your key rings by running the following
    `curl` command.
    {: #view-key-rings-params}

    ```sh
    $ curl -X GET \ "https://<region>.kms.cloud.ibm.com/api/v2/key_rings?totalCount=<show_total>&offset=<offset_value>&limit=<offset_limit>" \
        -H "accept: application/vnd.ibm.kms.key_ring+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

    Query parameters following the question mark **`?`** are optional, but included here to document their use.
    (: :note)
    
    Replace the variables in the example request according to the following
    table. 

|Variable|Description|
|--- |--- |
| region | **Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints). |
| IAM_token | **Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token). |
| instance_ID | **Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID). |
| correlation_ID | **Optional**. The unique identifier that is used to track and correlate transactions. |
| offset_limit | **Optional**. By default, `GET /key_rings` returns a sequence of 51 keyRings including the default keyRing. To retrieve a different set of key rings, use `limit` with `offset` to paginate through your available resources. The maximum value for `limit` is '5,000.' |
| offset_value | **Optional**. By specifying `offset`, you retrieve a subset of key rings that starts at the `offset` value. |
| show_total   | **Optional**. If set to `true`, the response metadata returns a value for `totalCount` in use with pagination.  |
{: caption="Table 3. Describes the variables that are needed to view key rings with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `GET api/v2/key_rings` request returns a collection of key
rings that are available in your
{{site.data.keyword.keymanagementserviceshort}} service instance.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key_ring+json",
        "collectionTotal": 2
    },
    "resources": [
        {
            "id": "default"
        },
        {
            "id": "Sample Key Ring 2",
            "creationDate": "2020-03-12T11:00:06Z",
            "createdBy": "..."
        }
    ]
}
```
{: screen}

## Deleting key rings with the API
{: #delete-key-ring-api}
{: api}

You can delete a key ring by making a `DELETE` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_id>
```

This action won't succeed if the key ring contains at least one key in a state other than the _Destroyed_ state. If the only keys in the key ring are in the _Destroyed_ state, the key ring can be deleted if `force=true` is added to the delete command. The keys in that state are automatically transferred to the `default` key ring.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key ring that you want to delete.

    You can find the ID for a key ring in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your key rings](/docs/key-protect?topic=key-protect-grouping-keys#list-key-ring-ap&interface=api#).

3. Run the following `curl` command to delete the key ring. Note the presence of `force=true`, which force deletes the key ring in the event that it contains keys in the _Destroyed_ state.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_id>?force=true" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "prefer: <return_preference>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ring_id|**Required.** The unique identifier for the key ring that you would like to delete.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 4. Describes the variables that are needed to delete keys with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful request returns an HTTP `204 No Content` response, which
indicates that the key ring was successfully deleted.


