---

copyright:
  years: 2020
lastupdated: "2020-11-09"

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

Key aliases are alternate names that can be used to identify a key. 

Each key can have up to five aliases. There is a limit of 1000 aliases per instance.
{: note}


## Creating key aliases with the API
{: #create-key-alias}

Create a key alias by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<alias>
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a key alias by running the following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/aliases/<key_alias>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.key+json" \
        -H "correlation-id: <correlation_ID>" \
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
            <strong>Required.</strong> A unique, human-readable name for easy
            identification of your key.
          </p>
          <p>
            Alias must be alphanumeric and cannot contain spaces or special 
            characters other than '-' or '_'. The alias cannot be a version 4 
            UUID and must not be a Key Protect reserved name: `allowed_ip`, `key`, 
            `keys`, `metadata`, `policy`, `policies`, `registration`, `registrations`, 
            `ring`, `rings`, `rotate`, `wrap`, `unwrap`, `rewrap`, `version`, `versions`.
            Alias size can be between 2 - 90 characters.
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
        Table 1. Describes the variables that are needed to create a key alias with
        the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    To protect the confidentiality of your personal data, avoid entering
    personally identifiable information (PII), such as your name or location,
    when you create a key alias. For more examples of PII, see section 2.2
    of the
    [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    A successful `POST api/v2/keys/<key_ID>/aliases/<key_alias>` response returns the ID value for your key,
    along with other metadata. The ID is a unique identifier that is assigned to
    your key and is used for subsequent calls to the
    {{site.data.keyword.keymanagementserviceshort}} API.

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
                "createdBy": "...",
            }
        ]
    }
    ```
    {: screen}

    For a detailed description of the response parameters, see the
    {{site.data.keyword.keymanagementserviceshort}}
    [REST API reference doc](/apidocs/key-protect){: external}.
    {: tip}

## Deleting key aliases with the API
{: #delete-key-alias}

Delete a key alias by making a `DELETE` call to the following endpoint.

```
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
        -H "correlation-id: <correlation_ID>" \
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
            <strong>Required.</strong> The unique, human-readable name that identifies
            your key.
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
        Table 2. Describes the variables that are needed to delete a key alias with
        the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>


    A successful `DELETE api/v2/keys/<key_ID>/aliases/<key_alias>` request returns an HTTP 
    `204 No Content` response, which indicates that the key alias associated with your key 
    was deleted. 
