---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Creating a transport key
{: #create-transport-keys}

You can enable the secure import of root key material to the cloud by first creating a transport encryption key for your {{site.data.keyword.keymanagementserviceshort}} service instance.
{: shortdesc}

Transport keys are used to encrypt and securely import root key material into {{site.data.keyword.keymanagementserviceshort}} based on the policies that you specify. To learn more about importing your keys securely to the cloud, see [Bringing your encryption keys to the cloud](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

Transport keys are currently a beta feature. Beta features can change at any time, and future updates might introduce changes are are incompatible with the latest version.
{: important}

## Creating a transport key with the API
{: #create-transport-key-api}

Create a transport key that's associated with your {{site.data.keyword.keymanagementserviceshort}} service instance by making a `POST` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Retrieve your service and authentication credentials to work with keys in the service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Set a policy for your transport key by calling the [{{site.data.keyword.keymanagementserviceshort}} API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

      <table>
        <tr>
          <th>Variable</th>
          <th>Description</th>
        </tr>
        <tr>
          <td><varname>region</varname></td>
          <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Regional service endpoints</a>.</td>
        </tr>
        <tr>
          <td><varname>IAM_token</varname></td>
          <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
        </tr>
        <tr>
          <td><varname>instance_ID</varname></td>
          <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
        </tr>
        <tr>
          <td><varname>expiration_time</varname></td>
          <td>
            <p>The time in seconds from the creation of a transport key that determines how long the key remains valid.</p>
            <p>The minimum value is 300 seconds (5 minutes), and the maximum value is 86400 (24 hours). The default value is 600 (10 minutes).</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>The number of times that a transport key can be retrieved within its expiration time before it is no longer accessible. The default value is 1.</td>
        </tr>
          <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to add a root key with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
      </table>

    A successful `POST api/v2/lockers` request creates a transport key for your service instance and returns its ID value, along with other metadata. The ID is a unique identifier that is associated to your transport key and is used for subsequent calls to the {{site.data.keyword.keymanagementserviceshort}} API.

3. Optional: Verify that the transport key was created by running the following call to retrieve metadata about your service instance.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## What's next
{: #create-transport-key-next-steps}

- To find out more about using a transport key to import root keys into the service, check out [Importing root keys](/docs/services/key-protect?topic=key-protect-import-root-keys).
- To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/key-protect){: new_window}.
