---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-18"

keywords: get details for a key, get key configuration, get details, view encryption key details, view encryption key, retrieve encryption key details, API examples

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

# Retrieving Key Metadata
{: #retrieve-key-metadata}

You can retrieve the general characteristics of a single encryption key by using
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Retrieving a key requires a _Writer_ or _Manager_ access policy, but you might
need a way to view only the details about a key, such as its transition history
or configuration, without retrieving the key itself. If you have _Reader_ access
permissions, you can use the {{site.data.keyword.keymanagementserviceshort}}
API to retrieve only metadata about a key.

## Viewing key metadata with the API
{: #view-key-metadata-api}

To view detailed information about a specific key, you can make a `GET` call to
the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID_or_alias>/metadata
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you would like to inspect.

    The ID value is used to access detailed information about the key. You can
    find the ID for a key in your
    {{site.data.keyword.keymanagementserviceshort}} instance by
    [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys),
    or by accessing the {{site.data.keyword.keymanagementserviceshort}}
    dashboard.

3. Get details about the key by running the following `curl` command.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID_or_alias>/metadata" \
        -H "accept: application/vnd.ibm.kms.key+json" \
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

      <tr>
        <td>
          <varname>key_ID_or_alias</varname>
        </td>
        <td>
          <strong>Required.</strong> The identifier or alias for the key that
          you want to inspect.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to view a details about
        a key with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    A successful `GET api/v2/keys/<key_ID_or_alias>/metadata` response returns
    details about your key. The following JSON object shows an example returned
    value for a standard key.

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
                "name": "test-standard-key",
                "aliases": [
                    "alias-1",
                    "alias-2"
                ],
                "state": 1,
                "expirationDate": "2020-03-15T03:50:12Z",
                "extractable": true,
                "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
                "imported": false,
                "creationDate": "2020-03-12T03:50:12Z",
                "createdBy": "...",
                "algorithmType": "AES",
                "algorithmMetadata": {
                    "bitLength": "256",
                    "mode": "CBC_PAD"
                },
                "algorithmBitSize": 256,
                "algorithmMode": "CBC_PAD",
                "lastUpdateDate": "2020-03-12T03:50:12Z",
                "dualAuthDelete": {
                    "enabled": false
                },
                "deleted": false
            }
        ]
    }
    ```
    {: screen}

    Need to retrieve the `payload` value for a standard key? To learn more, see
    [Retrieving a key](/docs/key-protect?topic=key-protect-retrieve-key).
    {: tip}

    For a detailed description of the response parameters, see the
    {{site.data.keyword.keymanagementserviceshort}}
    [REST API reference doc](/apidocs/key-protect){: external}.
