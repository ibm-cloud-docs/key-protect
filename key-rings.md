---

copyright:
  years: 2020
lastupdated: "2020-10-19"

keywords: key rings

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

# Managing key rings
{: #restore-keys}

You can use {{site.data.keyword.keymanagementservicefull}} to seperate the keys in your service instance.
{: shortdesc}

As an account admin, you can separate the keys in your {{site.data.keyword.keymanagementserviceshort}} 
service instance into groups called `key rings`. Key rings are a container of keys 
that belong to your service instance. You can grant access to the key rings within a
{{site.data.keyword.keymanagementserviceshort}} instance by using the
{{site.data.keyword.cloud_notm}} console.

Before you create a key ring for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **Pre-existing keys belong to the default key ring.**
  Each {{site.data.keyword.keymanagementserviceshort}} comes with a default key ring. The default
  key ring will hold all keys that existed prior to key ring assignment.

- **Key rings can hold standard and root keys.**
  Key rings can contain both standard and root keys.

- **A key can only belong to one key ring at a time.**
  A key can only belong to one key ring. Key ring assignment happens upon key creation. If a
  key ring id is not passed in upon creation, the key will belong to the default key ring.

- **It is not recommended to use key rings and fine grain policies within the same service instance.**
  A key can only belong to one key ring. Key ring assignment happens upon key creation. If a
  key ring id is not passed in upon creation, the key will belong to the default key ring.


## Creating key rings with the API
{: #create-key-ring-api}

Create a key ring by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a key ring by running the following cURL command.

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
          <varname>key_alias</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> A unique, human-readable name for easy
            identification of your key.
          </p>
          <p>
            <b>Important:</b> To protect your privacy, do not store your
            personal data as metadata for your key.
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>key_description</varname>
        </td>
        <td>
          <p>
            An extended description of your key.
          </p>
          <p>
            <b>Important:</b> To protect your privacy, do not store your
            personal data as metadata for your key.
          </p>
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
          format. The key will transition to the deactivated state within one
          hour past the key's expiration date.

          If the <code>expirationDate</code> attribute is omitted, the
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
            <code>false</code>, the service creates a root key that you can use
            for <code>wrap</code> or <code>unwrap</code> operations.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to add a root key with
        the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

The maximum amount of key rings is 50 per service instance.
{: note}

## Granting access to a key ring
{: #grant-access-key-ring}

You can grant access to a key ring within a
{{site.data.keyword.keymanagementserviceshort}} instance by using the
{{site.data.keyword.cloud_notm}} console.

Review
[roles and permissions](/docs/key-protect?topic=key-protect-manage-access)
to learn how {{site.data.keyword.cloud_notm}} IAM roles map to
{{site.data.keyword.keymanagementserviceshort}} actions.
{: tip}

To assign access:

1. From the menu bar, click **Manage** &gt; **Access (IAM)**, and select
   **Users** to browse the existing users in your account.
2. Select a table row, and click the ⋯ icon to open a list of options for that
   user.
3. From the options menu, click **Assign access**.
4. Click **Assign users additional access**.
5. Click the **IAM services** button.
6. From the list of services, select
   **{{site.data.keyword.keymanagementserviceshort}}**.
7. From the list of {{site.data.keyword.keymanagementserviceshort}} instances,
   select a {{site.data.keyword.keymanagementserviceshort}} instance that you
   want to grant access to.
8. Choose a combination of
   [platform and service access roles](/docs/key-protect?topic=key-protect-manage-access#roles)
   to assign access for the user.
9. Click **Add**.
10. Continue to add platform and service access roles as needed and when you are
    finished, click **Assign**.

## Listing key rings with the API
{: #list-key-ring-api}

-


## Deleting key rings with the API
{: #delete-key-ring-api}