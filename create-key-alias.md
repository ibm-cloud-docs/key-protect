---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-10" 

keywords: key alias, alias, key reference

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

# Creating key aliases
{: #create-key-alias}

You can use {{site.data.keyword.keymanagementservicefull}} to create a key alias with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}
{: api}

You can use {{site.data.keyword.keymanagementservicefull}} to create a key alias with the {{site.data.keyword.keymanagementserviceshort}} console.
{: shortdesc}
{: ui}

Key aliases are unique human-readable names that are references to a key that allows it to be identified and grouped beyond the limits of a display name. Aliases enable your service to refer to a key by recognizable custom names, rather than the auto-generated identifier provided by the {{site.data.keyword.keymanagementserviceshort}} service. For example, if you create a key that has the the ID `02fd6835-6001-4482-a892-13bd2085f75d` and it is aliased as `US-South-Test-Key`, you can use the `US-South-Test-Key` alias to refer to your key when you make calls to the {{site.data.keyword.keymanagementserviceshort}} api to [retrieve a key](/docs/key-protect?topic=key-protect-retrieve-key) or its [metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata). The alias can also be used to organize keys in the {{site.data.keyword.keymanagementserviceshort}} console.

## Creating and editing key aliases with the console
{: #create-key-alias-ui}
{: ui}

Key aliases can be added to a key during the process of creating or importing a key.

* For more information about creating a root key or a standard key, check out [Creating root keys](/docs/key-protect?topic=key-protect-create-root-keys) or [Creating standard keys](/docs/key-protect?topic=key-protect-create-standard-keys).
* For more information about importing a root key or a standard key, check out [Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys) or [Importing standard keys](/docs/key-protect?topic=key-protect-import-standard-keys).

To edit a key alias, click ⋯ and select **Edit key alias**. In the tab, you will see any existing aliases assigned to the key (and be able to delete them) and be able to add more alias. A key can have up to five aliases.

## Creating key aliases with the API
{: #create-key-alias-api}
{: api}

Create a key alias by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

   To create a key alias, you must be assigned a _Manager_ or _Writer_
   service access role. To learn how IAM roles map to
   {{site.data.keyword.keymanagementserviceshort}} service actions, check out
   [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
   {: note}

2. Create a key alias by running the following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<key_alias>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

Replace the variables in the example request according to the following
table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south`or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The identifier for the key that you would like to associate with an alias. To retrieve a key ID, see the [list keys API](/docs/key-protect?topic=key-protect-view-keys#retrieve-keys-api).|
|key_alias|**Required**. A unique, human-readable name for easy identification of your key.<br><br>Alias must be alphanumeric, case sensitive, and cannot contain spaces or special characters other than dashes (-) or underscores (_). The alias cannot be a version 4 UUID and must not be a {{site.data.keyword.keymanagementserviceshort}} reserved name: allowed_ip, key, keys, metadata, policy, policies, registration, registrations, ring, rings,rotate, wrap, unwrap, rewrap, version, versions.Alias size can be between 2 - 90 characters (inclusive).<br><br>Note You cannot have duplicate alias names in your {{site.data.keyword.keymanagementserviceshort}} instance.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see[Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
{: caption="Table 1. Describes the variables that are needed to create a key alias with the {{site.data.keyword.keymanagementserviceshort}} API" caption-side="top"}

To protect the confidentiality of your personal data, avoid entering
personally identifiable information (PII), such as your name or location,
when you create a key alias. For more examples of PII, see section 2.2
of the
[NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

A successful `POST api/v2/keys/<key_ID>/aliases/<key_alias>` response
returns the alias for your key, along with other metadata. The alias is a
unique name that is assigned to your key and can be used for to retrieve
more information about the associated key.

```json
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
    },
    "resources": [
        {
            "keyId": "02fd6835-6001-4482-a892-13bd2085f75d",
            "alias": "test-alias",
            "creationDate": "2020-03-12T03:37:32Z",
            "createdBy": "..."
        }
    ]
}
```
{: screen}

For a detailed description of the response parameters, see the
{{site.data.keyword.keymanagementserviceshort}}
[REST API reference doc](/apidocs/key-protect){: external}.
{: tip}

Each key can have up to five aliases. There is a limit of 1,000 aliases per
instance.
{: note}

## Deleting key aliases with the API
{: #delete-key-alias}
{: api}

Delete a key alias by making a `DELETE` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Delete a key alias by running the following `curl` command.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<key_alias>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
        -H "correlation-id: <correlation_ID>"
    ```
    {: codeblock}

Replace the variables in the example request according to the following
table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south`or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|key_ID|**Required**. The identifier for the key that you retrieved in [step 1](/docs/key-protect?topic=key-protect-view-keys#retrieve-keys-api).|
|key_alias|**Required**. The unique, human-readable name that identifies your key.|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
|correlation_ID|**Optional**.The unique identifier that is used to track and correlate transactions.|
{: caption="Table 2. Describes the variables that are needed to delete a key alias with the {{site.data.keyword.keymanagementserviceshort}} API" caption-side="top"}

A successful `DELETE api/v2/keys/<key_ID>/aliases/<key_alias>` request
returns an HTTP `204 No Content` response, which indicates that the alias
associated with your key was deleted.

It takes up to five minutes for an alias to be completely deleted from the service.
{: important}

## Key Alias FAQ
{: #alias-faq}

Below are additional details about key aliases:

- **An alias is independent from a key.**
  An alias is it's own resource and any actions taken on it will not affect the
  associated key. For example, deleting an alias will not delete the associated
  key.

- **An alias can only be associated with one key at a time.**
  An alias can only be associated with one key that is located in the same
  instance and region. If you would like to change the key that the alias is
  associated with, you will need to delete the alias, wait up to five minutes,
  then recreate the alias and map it to necessary key.

- **You can create an alias with the same name in a different instance or region.**
  Each alias will be associated with a different key in each instance or region.
  This enables your service's application code to be reusable in different
  instances or regions. For example, if you have an alias named
  `Application Key` in both the US-South and US-East regions, with each linked
  to a different key.

## APIs that use key alias
{: #key-alias-apis}
{: api}

The following table lists the APIs that you can use to create and use a key
alias.

| API | Key Alias Impact |
| --- | ---------------- |
| [Create Root Keys](/docs/key-protect?topic=key-protect-create-root-keys) | You can create up to 5 aliases while creating a root key. |
| [Create Standard Keys](/docs/key-protect?topic=key-protect-create-standard-keys) | You can create up to 5 aliases while creating a standard key. |
| [Retrieve a key](/docs/key-protect?topic=key-protect-retrieve-key) | You can retrieve a key by ID or alias. |
| [View key metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata) | You can retrieve the metadata of a key by ID or alias. |
{: caption="Table 3. Describes the variables that are APIs that use key alias." caption-side="top"}
