---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-22"

keywords: key versions, get key versions, list key versions

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

# Viewing key versions
{: #view-key-versions}

View the versions that are associated with a root key by using
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

When you rotate a root key, {{site.data.keyword.keymanagementserviceshort}}
creates a new version of the key. As a security admin, you can audit the
rotation history of a root key by viewing its key version history.

Key versions are available only for root keys. To learn more about how key
rotation works in {{site.data.keyword.keymanagementserviceshort}}, check out
[Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Viewing key versions with the API
{: #view-key-versions-api}

For a high-level view, you can list the versions that are associated with a root
key by making a `GET` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/{id}/versions
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the root key that you want to inspect.

    The ID value is used to access detailed information about the key. You can
    find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

3. Get a list of versions that are associated with the root key by running the
   following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/versions" \
        -H "accept: application/vnd.ibm.kms.key.version+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "x-kms-key-ring: <key_ring_ID>" \
        -H "bluemix-instance: <instance_ID>"
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
          want to inspect.
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
          <varname>key_ring_ID</varname>
        </td>
        <td>
          <p>
            <strong>Optional.</strong> The unique identifier of the key ring that the key belongs to. 
            If unspecified, {{site.data.keyword.keymanagementserviceshort}} will search for the key 
            in every key ring associated with the specified instance. It is recommended to specify 
            the key ring ID for a more optimized request.

            Note: The key ring ID of keys that are created without an `x-kms-key-ring` 
            header is: default.
          </p>
          <p>
            For more information, see
            [Managing key rings](docs/key-protect?topic=key-protect-managing-key-rings).
          </p>
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to list key versions
        with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    A successful `GET api/v2/keys/<key_ID>/versions` response returns the list
    of versions that are associated with the root key. The following JSON object
    shows an example returned value.

    ```json
    {
        "metadata": {
            "collectionType": "application/vnd.ibm.kms.key.version+json",
            "collectionTotal": 2
        },
        "resources": [
            {
                "id": "02fd6835-6001-4482-a892-13bd2085f75d",
                "creationDate": "2020-03-05T16:39:25Z"
            },
            {
                "id": "12e8c9c2-a162-472d-b7d6-8b9a86b815a6",
                "creationDate": "2020-03-02T16:28:38Z"
            }
        ]
    }
    ```
    {: screen}

    The `resources` object lists each key version, along with its ID and
    creation date, in reverse chronological order.
