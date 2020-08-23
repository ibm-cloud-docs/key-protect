---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-22"

keywords: create import token, secure import, key-wrapping key, import token API examples

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

# Creating import tokens
{: #create-import-tokens}

You can enable the secure import of root key material to the cloud by first
creating an import token for your
{{site.data.keyword.keymanagementserviceshort}} instance.
{: shortdesc}

Import tokens are used to encrypt and securely bring root key material into
{{site.data.keyword.keymanagementserviceshort}} based on the policies that you
specify. To learn more about importing your keys securely to the cloud, see
[Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys).

## Creating an import token with the API
{: #create-import-token-api}

Create an import token that's associated with your
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/import_token
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Set a policy for your import token by calling the
[{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/import_token" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/json" \
        -d '{
                "expiration": <expiration_time>,
                "maxAllowedRetrievals": <use_count>
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
            <strong>Required.</strong> The unique identifier that is assigned
            to your {{site.data.keyword.keymanagementserviceshort}} service
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
          <varname>expiration_time</varname>
        </td>
        <td>
          <p>
            The time in seconds from the creation of a import token that
            determines how long it remains valid.
          </p>
          <p>
            The minimum value is 300 seconds (5 minutes), and the maximum
            value is 86400 (24 hours). The default value is 600 (10 minutes).
          </p>
        </td>
      </tr>

      <tr>
        <td>
          <varname>use_count</varname>
        </td>
        <td>
          The number of times that an import token can be retrieved within its
          expiration time before it is no longer accessible. The default value
          is 1. The maximum value is 500.
        </td>
      </tr>

      <caption style="caption-side:bottom;">
        Table 1. Describes the variables that are needed to create an import
        token with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    A successful `POST api/v2/import_token` request creates an import token for
    your {{site.data.keyword.keymanagementserviceshort}} instance. The
    response body contains the metadata that is associated with your import
    token, such as its creation date and policy details. The following snippet
    shows example output.

    You can only have 1 import token associated with your
    {{site.data.keyword.keymanagementserviceshort}} instance at any given time.
    Any subsequent create import token requests will override the previous
    import token.
    {: note}

    ```json
    {
        "creationDate": "2019-04-08T16:58:29Z",
        "expirationDate": "2019-04-08T17:18:29Z",
        "maxAllowedRetrievals": 1,
        "remainingRetrievals": 1
    }
    ```
    {: screen}

## Retrieving an import token with the API
{: #retrieve-import-token-api}

Retrieve the import token that's associated with your
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/import_token
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the import token that is associated with your
   {{site.data.keyword.keymanagementserviceshort}} instance by calling the
   [{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect){: external}.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/import_token" \
        -H "authorization: Bearer <IAM_token>" \
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
        Table 1. Describes the variables that are needed to retrieve an import
        token with the {{site.data.keyword.keymanagementserviceshort}} API
      </caption>
    </table>

    A successful `GET api/v2/import_token` request retrieves the import token
    for your {{site.data.keyword.keymanagementserviceshort}} instance. The
    response body contains the metadata that is associated with your import
    token, such as its creation date and policy
    details.

    The retrieved import token can be reused to import one or more keys up until
    the date that the import token expires.
    {: note}

    The following snippet shows example output with truncated values.

    ```json
    {
        "creationDate": "2019-04-08T16:58:29Z",
        "expirationDate": "2019-04-08T17:18:29Z",
        "maxAllowedRetrievals": 1,
        "remainingRetrievals": 0,
        "payload": "Rm91ciBzY29yZSBhbmQgc2V2ZW4geWVhcnMgYWdv",
        "nonce": "8zJE9pKVdXVe/nLb"
    }
    ```
    {: screen}

    The response body also contains the public encryption key that you can use
    to
    [encrypt a root key](/docs/key-protect?topic=key-protect-importing-keys#using-import-tokens)
    before you upload the key material to a
    {{site.data.keyword.keymanagementserviceshort}} instance.

    In the example, the `payload` value represents the public key that is
    associated with the import token. This value is base64 encoded. For extra
    security, {{site.data.keyword.keymanagementserviceshort}} also provides a
    `nonce` value that is used to verify the originality of a key import request
    to the service. To learn more about how to use these values, check out
    [Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).

## What's next
{: #create-import-token-next-steps}

- To find out more about using an import token to securely bring encryption keys
to {{site.data.keyword.cloud_notm}}, check out
[Importing root keys](/docs/key-protect?topic=key-protect-import-root-keys).
- For a guided tutorial on using import tokens in
{{site.data.keyword.keymanagementserviceshort}}, see
[Tutorial: Creating and importing encryption keys](/docs/key-protect?topic=key-protect-tutorial-import-keys).
