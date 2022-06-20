---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-20"

keywords: list keys, view keys, retrieve encryption key

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

# Viewing a list of keys
{: #view-keys}

{{site.data.keyword.keymanagementservicefull}} provides a centralized system to view, manage, and audit your encryption keys. Audit your keys and access restrictions to keys to ensure the security of your resources.
{: shortdesc}

While you can [assign fine-grained access to a single key](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level), note that calling the [list keys API](/apidocs/key-protect#getkeys) will not return keys that you have assigned individual access to (that only you can access, in other words). Calling this API will however return the keys in key rings you have access to (if you have access to all of the keys in an instance, you will see all keys). You can, however, see the keys that only you have access to by following the instructions in [Viewing fine-grain access keys through IAM](#filter-key-state-gui-iams) to view the key through IAM or by using the API to pass the specific key ID.
{: important}

It is a good practice to audit your key configuration regularly:

- Examine when keys were created and determine whether it's time to rotate the key.

- [Monitor API calls to {{site.data.keyword.keymanagementserviceshort}} with {{site.data.keyword.at_short}}](/docs/key-protect?topic=key-protect-at-events).

- Inspect which users have access to keys and if the level of access is appropriate.

For more information about auditing access to your resources, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).

## Viewing keys in the console
{: #view-keys-gui}
{: ui}

If you prefer to inspect the keys in your service by using a graphical interface, you can use the {{site.data.keyword.keymanagementserviceshort}} dashboard.

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys), complete the following steps to view your keys.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** > **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click on **Keys**, which shows a list of all keys in your service instance. Keys can be filtered by their **Key states** (for example, to show only keys in the **Enabled** state) or by their **Key ring ID** using the drop-down lists. The search bar can be used to search keys by their display name, key ID, and alias. Note that the quickest way to find a key is to search by its key ID. The fields found in the table can be customized using the **Settings** button. By default you will see:

| Column | Description |
| ------ | ----------- |
| Name   | The display name you gave to your key. |
| ID     | A unique key ID that was assigned to your key by the {{site.data.keyword.keymanagementserviceshort}} service. You can use the ID value to make calls to the service with the [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}. |
| Key alias | The [key alias](/docs/key-protect?topic=key-protect-create-key-alias) (or aliases) of the key. |
| Key ring ID | The [key ring](/docs/key-protect?topic=key-protect-grouping-keys) the keys are associated with. These states include _Deactivated_, _Destroyed_, _Disabled_, and _Enabled_. |
| Last updated | The date the last time the key was updated. This field can be particularly helpful when assessing whether a _Destroyed_ key can be restored, since restorations can only take place within 30 days of a key being placed in the _Destroyed_ state. |
| Created | The date the key was created. |
| Type   | The [key type](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) of the key (either a Root key or a Standard key). |
| Key states | The [key state](/docs/key-protect?topic=key-protect-key-states) of the key, one of _Deactivated_, _Destroyed_, _Disabled_, or _Enabled_. |
{: caption="Table 1. Describes the Keys table." caption-side="bottom"}

If you have more than 5,000 keys, and you cannot filter the number of keys that will be searched to less than 5,000 (for example, by filtering by key state to only look for `Enabled` keys), your search will fail unless it exactly matches a key ID or alias. For more information about the API spec for key search, check out [GET /keys](/apidocs/key-protect#getkeys).
{: tip}

If you want to narrow the number of results returned by a search, try using one or a combination of the following parameters:

* `not:` when specified, inverts the logic the search uses (for example, `not:foo` will search for keys that have aliases or names that do not contain `foo`).
* `escape:` everything after this option is take as plaintext (example: `escape:not:` will search for keys that have an alias or name containing the substring `not:`).
* `exact:` only looks for exact matches.
* `alias:` only looks for key aliases.
* `name:` only looks for key names.

Note that `not:exact:foobar` will look for keys where key name or alias is *not* exactly `foobar`, while `exact:not:foobar` will look for keys where key name or alias is exactly `not:foobar`

Search scopes behave in an *OR* manner. This means when using more than one search scope, a match in at least one of the scopes will result in the key being returned. By default (if no scopes are provided), the search is performed in both `name` and `alias` scopes.
{: important}

**Not seeing the full list of keys that are stored in your {{site.data.keyword.keymanagementserviceshort}} instance?** Verify with your administrator that you are assigned the correct role for the applicable {{site.data.keyword.keymanagementserviceshort}} instance or individual key. For more information about roles, see [Roles and permissions](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).

### Retrieving keys by state
{: #filter-key-state-gui}
{: ui}

By filtering on the state of specific keys in your {{site.data.keyword.keymanagementserviceshort}} instance, you can retrieve keys that are in the states that you specify.

For example, you might have keys in your {{site.data.keyword.keymanagementserviceshort}} instance that are in the active, suspended, and destroyed states, but you only want to retrieve keys in the active state when you look through a list of keys.

For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys), you have two options to view your keys. The first option, [View keys through the resource list](#filter-key-state-gui-resource-list), will work for all keys except those you have assigned [fine-grained access to](/docs/key-protect?topic=key-protect-grant-access-keys#grant-access-key-level). For information about how to view keys with fine-grain access, check out [Viewing fine-grain access keys IAM](#filter-key-state-gui-iam).

#### Viewing keys through the resource list
{: #filter-key-state-gui-resource-list}
{: ui}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, click the filter icon and select the dropdown from the **Status** menu.

5. Select the key state of the keys that you would like to retrieve.

6. Click the **Apply** button.

#### Viewing fine-grain access keys through IAM
{: #filter-key-state-gui-iam}
{: ui}

1. From the menu bar, click **Manage** &gt; **Access (IAM)**, and select **Users** to browse the existing users in your account.

2. Select a table row, and click the ⋯ icon to open a list of options for that user. Then select **Manage Access** from the drop down list.

3. Here you can see all of the IAM information for this user, including the Access groups they belong to. To specifically see the access policies for this user, click the **Access policies** tab.

An account owner or user with the appropriate privileges can see all of the policies assigned to this user, including any fine-grained access over keys.

## Viewing keys with the API
{: #view-keys-api}
{: api}

You can retrieve the contents of your keys by using the {{site.data.keyword.keymanagementserviceshort}} API.

### Retrieving a list of keys
{: #retrieve-keys-api}
{: api}

For a high-level view, you can browse keys that are managed in your provisioned instance of {{site.data.keyword.keymanagementserviceshort}} by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. View general characteristics about your keys by running the following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the information in Table 1. For more information about optional parameters available when viewing collections of keys, including the ability to search your keys, see the [API documentation regarding the `List keys` method](/apidocs/key-protect#getkeys).

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as us-south or eu-gb, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides. For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request. For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} instance. For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|key_ring_ID|**Optional**. The unique identifier of the target key ring. If unspecified, the response will include all resources that the user has access to in the specified instance. If provided, the response will only include resources that the user has access to in the specified key ring. For more information, see [Grouping keys](/docs/key-protect?topic=key-protect-grouping-keys).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
{: caption="Table 1. Describes the variables that are needed to view keys with the {{site.data.keyword.keymanagementserviceshort}} API." caption-side="top"}

A successful `GET api/v2/keys` request returns a collection of keys that are available in your {{site.data.keyword.keymanagementserviceshort}} service instance. 

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 2
    },
    "resources": [
        {
            "id": "02fd6835-6001-4482-a892-13bd2085f75d",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Root-key",
            "state": 1,
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "createdBy": "...",
            "creationDate": "2020-03-11T16:30:06Z",
            "lastUpdateDate": "2020-03-11T16:30:06Z",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "Deprecated"
            },
            "extractable": false,
            "imported": true,
            "algorithmMode": "Deprecated",
            "algorithmBitSize": 256,
            "dualAuthDelete": {
                "enabled": false
            }
        },
        {
            "id": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Standard-key",
            "state": 1,
            "expirationDate": "2020-03-14T03:50:12Z",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:30372f20-d9f1-40b3-b486-a709e1932c9c:key:2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "createdBy": "...",
            "creationDate": "2020-03-12T03:50:12Z",
            "lastUpdateDate": "2020-03-12T03:50:12Z",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "Deprecated"
            },
            "extractable": true,
            "imported": false,
            "algorithmMode": "Deprecated",
            "algorithmBitSize": 256,
            "dualAuthDelete": {
                "enabled": false
            }
        }
    ]
}
```
{: screen}

By default, `GET api/v2/keys` returns your first 200 keys, but you can adjust this limit by using the `limit` parameter at query time. To learn more about `limit` and `offset`, see [Retrieving a subset of keys](#retrieve-subset-keys-api).

Not seeing the full list of keys? You might need to use `limit` and `offset` or check with your administrator to ensure you're assigned the correct level access to keys in your instance. To learn more, see [Unable to view or list keys](/docs/key-protect?topic=key-protect-troubleshooting#unable-to-list-keys-api).
{: tip}

### Retrieving a subset of keys
{: #retrieve-subset-keys-api}
{: api}

By specifying the `limit` and `offset` parameters at query time, you can retrieve a subset of your keys, starting with the `offset` value that you specify.

For example, you might have 3000 total keys that are stored in your {{site.data.keyword.keymanagementserviceshort}} instance, but you want to retrieve keys 200 - 300 when you make a `GET /keys` request.

You can use the following example request to retrieve a different set of keys.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys?offset=<offset>&limit=<limit>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `limit` and `offset` variables in your request according to the following table.

|Variable|Description|
|--- |--- |
|offset|The number of keys to skip. For example, if you have 50 keys in your instance, and you want to list keys 26 - 50, use `../keys?offset=25`. You can also pair offset with limit to page through your available resources.|
|limit|The number of keys to retrieve. For example, if you have 100 keys in your instance, and you want to list only 10 keys, use `../keys?limit=10`. The maximum value for limit is 5000.|
{: caption="Table 2. Table 2. Describes usage of the limit and offset variables." caption-side="top"}

Offset is the location of a particular key in a data set. The `offset` value is zero-based, which means that the 10th encryption key in a data set is at offset 9.
{: tip}

### Retrieving keys by state
{: #filter-keys-state-api}
{: api}

By specifying the `state` parameter at query time, you can retrieve keys that are in the states that you specify.

For example, you might have keys in your {{site.data.keyword.keymanagementserviceshort}} instance that are in the active, suspended, and destroyed states, but you only want to retrieve keys in the active state when you make a `GET /keys` request.

The state query parameter takes in a list of integers from 0 to 5 delimited by commas with no whitespace or trailing commas. For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

You can use the following example request to retrieve a different set of keys.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys?state=<state_integers>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `state` variable in your request according to the following table.

|Variable|Description|
|--- |--- |
|state|The states of the keys to be retrieved. States are integers, where Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, and Destroyed = 5 values. For example, if you want to only list keys in the active state in your {{site.data.keyword.keymanagementserviceshort}} instance, use `../keys?state=1`. You can also pair states with offsets and limits to page through your available resources.|
{: caption="Table 3. Describes the state variable." caption-side="top"}

For usage notes, check out the following examples for setting your `state` query parameter.

|URL|Description|
|--- |--- |
|`.../keys`|Lists all of your available resources, up to the first 200 keys.|
|`.../keys?state=5`|Lists keys in the deleted state.|
|`.../keys?state=2,3`|Lists keys in the suspended and deactivated state.|
{: caption="Table 4. Provides usage notes for the stage query parameter." caption-side="top"}

### Retrieving keys by Extractable value
{: #filter-keys-extractable-state-api}
{: api}

By specifying the `extractable` parameter at query time, you can retrieve keys whose material can leave the service.

For example, you might have both standard and root keys in your {{site.data.keyword.keymanagementserviceshort}} instance, but you only want to retrieve keys with extractable key material when you make a `GET /keys` request.

The extractable query parameter takes in a boolean.
{: note}

You can use the following example request to retrieve a different set of keys.

```sh
$ curl -X GET \
    "https://<region>.kms.cloud.ibm.com/api/v2/keys?extractable=<extractable>" \
    -H "accept: application/vnd.ibm.collection+json" \
    -H "authorization: Bearer <IAM_token>" \
    -H "bluemix-instance: <instance_ID>"
```
{: codeblock}

Replace the `extractable` variable in your request according to the following table.

|Variable|Description|
|--- |--- |
|extractable|The type of keys to be retrieved. Filters keys based on the extractable property. You can use this query parameter to search for keys whose material can leave the service. If set to true, standard keys will be retrieved. If set to false, root keys will be retrieved. If omitted, both root and standard keys will be retrieved. For example, if you want to only list keys with extractable material in your {{site.data.keyword.keymanagementserviceshort}} instance, use `../keys?extractable=true`. You can also pair extractable with `offset`, `limit`, and `state` to page through your available resources.|
{: caption="Table 5. Describes the extractable variable." caption-side="top"}

For usage notes, check out the following examples for setting your `extractable` query parameter.

|URL|Description|
|--- |--- |
|`../keys`|Lists all of your available resources, up to the first 200 keys.|
|`../keys?extractable=true`|Lists standard keys.|
|`../keys?extractable=false`|Lists root keys.|
{: caption="Table 6. Provides usage notes for the extractable query parameter." caption-side="top"}
