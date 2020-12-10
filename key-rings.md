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

For a high-level view, you can browse the key rings that are managed in your 
provisioned instance of {{site.data.keyword.keymanagementserviceshort}} by 
making a GET call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).


2. View general characteristics about your keys by running the following cURL
   command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys" \
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
    Table 2. Describes the variables that are needed to view keys with the
    {{site.data.keyword.keymanagementserviceshort}} API.
  </caption>
</table>

    A successful `GET api/v2/keys` request returns a collection of keys that are
    available in your {{site.data.keyword.keymanagementserviceshort}} service
    instance.

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
                    "mode": "CBC_PAD"
                },
                "extractable": false,
                "imported": true,
                "algorithmMode": "CBC_PAD",
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
                    "mode": "CBC_PAD"
                },
                "extractable": true,
                "imported": false,
                "algorithmMode": "CBC_PAD",
                "algorithmBitSize": 256,
                "dualAuthDelete": {
                    "enabled": false
                }
            }
        ]
    }
    ```
    {: screen}


## Deleting key rings with the API
{: #delete-key-ring-api}

You can delete a key and its contents by making a
`DELETE` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>
```

This action won't succeed if the key is actively protecting one or more cloud
resources. You can
[review the resources](/docs/key-protect?topic=key-protect-view-protected-resources)
that are associated with the key, or
[use the `force` parameter](#delete-key-force)
at query time to delete the key.
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you want to delete.

    You can find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

3. Run the following cURL command to delete the key and its contents.

    ```sh
    $ curl -X DELETE \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>" \
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
          <varname>key_ID</varname>
        </td>
        <td>
          <strong>Required.</strong> The unique identifier for the key that you
          would like to delete.
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
          <varname>return_preference</varname>
        </td>
        <td>
          <p>
            A header that alters server behavior for <code>POST</code> and
            <code>DELETE</code> operations.
          </p>
          <p>
            When you set the <em>return_preference</em> variable to
            <code>return=minimal</code>, the service returns a successful
            deletion response. When you set the variable to
            <code>return=representation</code>, the service returns both the key
            material and the key metadata.
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to delete keys with the
        {{site.data.keyword.keymanagementserviceshort}} API.
      </caption>
    </table>

    If the `return_preference` variable is set to `return=representation`, the
    details of the `DELETE` request are returned in the response entity-body.

    After you delete a key, it transitions to the `Deactivated` key state. After
    24 hours, if a key is not reinstated, the key transitions to the `Destroyed`
    state. The key contents are permanently erased and no longer accessible.

    The following JSON object shows an example returned value.

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
                "name": "test-root-key",
                "state": 5,
                "extractable": false,
                "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
                "imported": false,
                "creationDate": "2020-03-10T20:41:27Z",
                "createdBy": "...",
                "algorithmType": "AES",
                "algorithmMetadata": {
                    "bitLength": "256",
                    "mode": "CBC_PAD"
                },
                "algorithmBitSize": 256,
                "algorithmMode": "CBC_PAD",
                "lastUpdateDate": "2020-03-16T20:41:27Z",
                "dualAuthDelete": {
                    "enabled": false
                },
                "deleted": true,
                "deletionDate": "2020-03-16T21:46:53Z",
                "deletedBy": "..."
            }
        ]
    }
    ```
    {: screen}

    For a detailed description of the available parameters, see the
    {{site.data.keyword.keymanagementserviceshort}}
    [REST API reference doc](/apidocs/key-protect){: external}.