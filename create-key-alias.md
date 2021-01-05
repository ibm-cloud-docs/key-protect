---

copyright:
  years: 2020
lastupdated: "2020-11-17"

keywords: Create key alias, key alias, view encryption key, retrieve encryption key by alias, create alias API examples

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

# Creating key aliases
{: #create-key-alias}

You can use {{site.data.keyword.keymanagementservicefull}} to create a key alias
with the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

Key aliases are unique human-readable names that can be used to identify a key.
Aliases enable your service to refer to a key by recognizable custom names,
rather than the auto-generated identifier provided by the
{{site.data.keyword.keymanagementserviceshort}} service. For example, if you
create a key that has the the ID `02fd6835-6001-4482-a892-13bd2085f75d` and
it is aliased as `US-South-Test-Key`, you can use `US-South-Test-Key` to
refer to your key when you make calls to the
{{site.data.keyword.keymanagementserviceshort}} api to
[retrieve a key](/docs/key-protect?topic=key-protect-retrieve-key) or
its [metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata).

## Creating key aliases with the API
{: #create-key-alias-api}

Create a key alias by making a `POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

   To create a key alias, you must be assigned a _Manager_ or _Writer_
   service access role. To learn how IAM roles map to
   {{site.data.keyword.keymanagementserviceshort}} service actions, check out
   [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
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
            {{site.data.keyword.keymanagementserviceshort}} instance
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
          <strong>Required.</strong> The identifier for the key that you
          would like to associate with an alias. To retrieve a key ID, see the
          [list keys API](/docs/key-protect?topic=key-protect-view-keys#retrieve-keys-api).
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_alias</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> A unique, human-readable name for easy
            identification of your key.
          </p>
          <p>
            Alias must be alphanumeric, case sensitive, and cannot contain
            spaces or special characters other than dashes (<code>-</code>) or
            underscores (<code>_</code>). The alias cannot be a version 4 UUID
            and must not be a {{site.data.keyword.keymanagementserviceshort}}
            reserved name: <code>allowed_ip</code>, <code>key</code>,
            <code>keys</code>, <code>metadata</code>, <code>policy</code>,
            <code>policies</code>, <code>registration</code>,
            <code>registrations</code>, <code>ring</code>, <code>rings</code>,
            <code>rotate</code>, <code>wrap</code>, <code>unwrap</code>,
            <code>rewrap</code>, <code>version</code>, <code>versions</code>.
            Alias size can be between 2 - 90 characters (inclusive).
          </p>
          <p>
            <strong>Note</strong> You cannot have duplicate alias names in your
            {{site.data.keyword.keymanagementserviceshort}} instance.
          </p>
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
            token, including the Bearer value, in the <code>curl</code> request.
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

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to create a key alias
        with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

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
            {{site.data.keyword.keymanagementserviceshort}} instance
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
          <strong>Required.</strong> The identifier for the key that you
          retrieved in
          [step 1](/docs/key-protect?topic=key-protect-view-keys#retrieve-keys-api).
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_alias</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique, human-readable name that
            identifies your key.
          </p>
      </tr>

      <tr>
        <td>
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the <code>curl</code> request.
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

      <caption style="caption-side:bottom;">
        Table 2. Describes the variables that are needed to delete a key alias
        with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    A successful `DELETE api/v2/keys/<key_ID>/aliases/<key_alias>` request
    returns an HTTP `204 No Content` response, which indicates that the alias
    associated with your key was deleted.

    It takes up to 10 minutes for an alias to be completely deleted from the
    service.
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
  associated with, you will need to delete the alias, wait up to 10 minutes,
  then recreate the alias and map it to necessary key.

- **You can create an alias with the same name in a different instance or region.**
  Each alias will be associated with a different key in each instance or region.
  This enables your service's application code to be reusable in different
  instances or regions. For example, if you have an alias named
  `Application Key` in both the US-South and US-East regions, with each linked
  to a different key.

## APIs that use key alias
{: #key-alias-apis}

The following table lists the APIs that you can use to create and use a key
alias.

| API | Key Alias Impact |
| --- | ---------------- |
| [Create Root Keys](/docs/key-protect?topic=key-protect-create-root-keys) | You can create up to 5 aliases while creating a root key. |
| [Create Standard Keys](/docs/key-protect?topic=key-protect-create-standard-keys) | You can create up to 5 aliases while creating a standard key. |
| [Retrieve a key](/docs/key-protect?topic=key-protect-retrieve-key) | You can retrieve a key by ID or alias. |
| [View key metadata](/docs/key-protect?topic=key-protect-retrieve-key-metadata) | You can retrieve the metadata of a key by ID or alias. |
{: caption="Table 3. Describes the variables that are APIs that use key alias." caption-side="top"}
