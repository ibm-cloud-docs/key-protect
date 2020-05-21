---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

keywords: create standard encryption key, create secret, persist secret, create encryption key, standard encryption key API examples

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

# Creating standard keys
{: #create-standard-keys}

You can create a standard encryption key with the
{{site.data.keyword.keymanagementserviceshort}} GUI, or programmatically with
the {{site.data.keyword.keymanagementserviceshort}} API.
{: shortdesc}

## Creating standard keys in the console
{: #create-standard-key-gui}

[After you create an instance of the service](/docs/key-protect?topic=key-protect-provision),
complete the following steps to create a standard key in the
{{site.data.keyword.cloud_notm}} console.

If you enable
[dual authorization settings for your service instance](/docs/key-protect?topic=key-protect-manage-dual-auth),
keep in mind that any keys that you add to the service require an authorization
from two users to delete keys.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your
provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. To create a new key, click **Add key** and select the **Create a key**
window.

    Specify the key's details:

    <table>
      <tr>
        <th>Setting</th>
        <th>Description</th>
      </tr>

      <tr>
        <td>
          Name
        </td>
        <td>
          <p>
            A unique, human-readable alias for easy identification of your key.
          </p>
          <p>
            To protect your privacy, ensure that the key name does not contain
            personally identifiable information (PII), such as your name or
            location.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          Key type
        </td>
        <td>
          The
          [type of key](/docs/key-protect/concepts?topic=key-protect-envelope-encryption#key-types)
          that you would like to manage in
          {{site.data.keyword.keymanagementserviceshort}}. From the list of key
          types, select <b>Standard key</b>.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the <b>Create a key</b> settings
      </caption>
    </table>

5. When you are finished filling out the key's details, click **Create key** to
confirm.

## Creating standard keys with the API
{: #create-standard-key-api}

Create a standard key by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Call the
[{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect){: external}
with the following cURL command.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
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
            "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
            "extractable": <key_type>
          }
        ]
      }'
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
            {{site.data.keyword.keymanagementserviceshort}} service instance
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
          <varname>IAM_token</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}}
            access token. Include the full contents of the <code>IAM</code>
            token, including the Bearer value, in the cURL request.
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

      <tr>
        <td>
          <varname>return_preference</varname>
        </td>
        <td>
          <p>
            A header that alters server behavior for <code>POST</code> and
            <code>DELETE</code> operations.
          </p>
          <p>
            When you set the <em>return_preference</em> variable to
            <code>return=minimal</code>, the service returns only the key
            metadata, such as the key name and ID value, in the response
            entity-body. When you set the variable to
            <code>return=representation</code>, the service returns both the key
            material and the key metadata.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_alias</varname>
        </td>
        <td>
          <strong>Required.</strong> A unique, human-readable name for easy
          identification of your key. To protect your privacy, do not store your
          personal data as metadata for your key.
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_description</varname>
        </td>
        <td>
          An extended description of your key. To protect your privacy, do not
          store your personal data as metadata for your key.
        </td>
      </tr>

      <tr>
        <td>
          <varname>YYYY-MM-DD</varname>
          <br>
          <varname>HH:MM:SS.SS</varname>
        </td>
        <td>
          The date and time that the key expires in the system, in RFC 3339
          format. If the <code>expirationDate</code> attribute is omitted, the
          key does not expire.
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_type</varname>
        </td>
        <td>
          <p>
            A boolean value that determines whether the key material can leave
            the service.
          </p>
          <p>
            When you set the <code>extractable</code> attribute to
            <code>true</code>, the service creates a standard key that you can
            store in your apps or services.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 2. Describes the variables that are needed to add a standard key
        with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    To protect the confidentiality of your personal data, avoid entering
    personally identifiable information (PII), such as your name or location,
    when you add keys to the service.For more examples of PII, see section 2.2
    of the
    [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    A successful `POST api/v2/keys` response returns the ID value for your key,
    along with other metadata. The ID is a unique identifier that is assigned to
    your key and is used for subsequent calls to the
    {{site.data.keyword.keymanagementserviceshort}} API.

3. Optional: Verify that the key was created by running the following call to
get the keys in your {{site.data.keyword.keymanagementserviceshort}} service
instance.

    ```cURL
    curl -X GET \
      https://<regon>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## What's next
{: #create-standard-key-next-steps}

- To find out more about programmatically managing your keys,
[check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](https://{DomainName}/apidocs/key-protect){: external}.
