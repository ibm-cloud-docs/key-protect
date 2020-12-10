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

You can use {{site.data.keyword.keymanagementservicefull}} to separate the keys in your service instance.
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
  Each newly created {{site.data.keyword.keymanagementserviceshort}} comes with a generated key ring 
  with an ID of `default`. All keys that are not associated with an otherwise specified key ring exists
  within the default key ring..

- **Key rings can hold standard and root keys.**
  Key rings can contain both standard and root keys. There is not a limit on how many keys can exist
  within a key ring.

- **A key can only belong to one key ring at a time.**
  A key can only belong to one key ring. Key ring assignment happens upon key creation. If a
  key ring id is not passed in upon creation, the key will belong to the default key ring.

- **It is not recommended to use key rings and fine grain policies within the same service instance.**


- **You should remove all keys from a key ring before deleting it.**


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
        "https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_id>" \
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
          <varname>key_ring_id</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier for the key ring 
            that you would like to create.
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

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to create a key ring with
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

For a high-level view, you can browse the key rings that are managed in your 
provisioned instance of {{site.data.keyword.keymanagementserviceshort}} by 
making a GET call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys_rings
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).


2. View general characteristics about your key rings by running the following cURL
   command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/key_rings" \
        -H "accept: application/vnd.ibm.collection+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
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
        {{site.data.keyword.keymanagementserviceshort}} instance resides.
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
        <strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access
        token. Include the full contents of the <code>IAM</code> token,
        including the Bearer value, in the cURL request.
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
        your {{site.data.keyword.keymanagementserviceshort}} instance.
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
      <p>
        The unique identifier that is used to track and correlate transactions.
      </p>
    </td>
  </tr>

  <caption style="caption-side:bottom;">
    Table 2. Describes the variables that are needed to view key rings with the
    {{site.data.keyword.keymanagementserviceshort}} API.
  </caption>
</table>

    A successful `GET api/v2/key_rings` request returns a collection of key rings 
    that are available in your {{site.data.keyword.keymanagementserviceshort}} service
    instance.

    ```json
    {
        "metadata": {
            "collectionType": "application/vnd.ibm.kms.key+json",
            "collectionTotal": 2
        },
        "resources": [
            {
                "id": "Sample Key Ring",
                "creationDate": "2020-03-11T16:30:06Z",
                "createdBy": "...",
            },
            {
                "id": "Sample Key Ring 2",
                "creationDate": "2020-03-12T11:00:06Z",
                "createdBy": "...",
            },
        ]
    }
    ```
    {: screen}


## Deleting key rings with the API
{: #delete-key-ring-api}

You can delete a key ring by making a
`DELETE` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_id>
```

This action won't succeed if the key ring contains at least one key, regardless of key state. 
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key ring that you want to delete.

    You can find the ID for a key ring in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your key rings](#list-key-ring-api).

3. Run the following cURL command to delete the key ring.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/key_rings/<key_ring_ID>" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "prefer: <return_preference>"
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
          <varname>key_ring_id</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The unique identifier for the key ring 
            that you would like to delete.
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


      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to delete keys with the
        {{site.data.keyword.keymanagementserviceshort}} API.
      </caption>
    </table>

    A successful request returns an HTTP `204 No Content` response, which
    indicates that your key ring was deleted.