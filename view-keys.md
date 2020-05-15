---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-15"

keywords: list encryption keys, view encryption key, retrieve encryption key, retrieve key API examples

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

# Viewing a list of keys
{: #view-keys}

{{site.data.keyword.keymanagementservicefull}} provides a centralized system to view, manage, and audit your encryption keys. Audit your keys and access r
estrictions to keys to ensure the security of your resources.
{: shortdesc}

Audit your key configuration regularly:

- Examine when keys were created and determine whether it's time to rotate the key.
- [Monitor API calls to {{site.data.keyword.keymanagementserviceshort}} with {{site.data.keyword.cloudaccesstrailshort}}](/docs/key-protect?topic=key-protect-at-events).
- Inspect which users have access to keys and if the level of access is appropriate.

For more information about auditing access to your resources, see [Managing user access](/docs/key-protect?topic=key-protect-manage-access).

## Viewing keys in the console
{: #view-keys-gui}

If you prefer to inspect the keys in your service by using a graphical interface, you can use the {{site.data.keyword.keymanagementserviceshort}} dashboard.

[After you create or import your existing keys into the service](/docs/key-protect?topic=key-protect-create-root-keys), complete the following steps to view your keys.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/).
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. Browse the general characteristics of your keys from the application details page:

    <table>
      <tr>
        <th>Column</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>The unique, human-readable alias that was assigned to your key.</td>
      </tr>
      <tr>
        <td>ID</td>
        <td>A unique key ID that was assigned to your key by the {{site.data.keyword.keymanagementserviceshort}} service. You can use the ID value to make 
        calls to the service with the [{{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect).</td>
      </tr>
      <tr>
        <td>State</td>
        <td>The [key state](/docs/key-protect?topic=key-protect-key-states) based on 
        [NIST Special Publication 800-57, Recommendation for Key Management](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0). 
        These states include <i>Pre-active</i>, <i>Active</i>, <i>Deactivated</i>, and 
        <i>Destroyed</i>.</td>
      </tr>
      <tr>
        <td>Type</td>
        <td>The [key type](/docs/key-protect?topic=key-protect-envelope-encryption#key-types) that describes your key's designated purpose within the 
        service.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the <b>Keys</b> table</caption>
    </table>

    Not seeing the full list of keys that are stored in your service instance? Verify with your administrator that you are assigned the correct role for the 
    applicable service instance or 
    individual key. For more information about roles, see [Roles and permissions](/docs/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

## Viewing keys with the API
{: #view-keys-api}

You can retrieve the contents of your keys by using the {{site.data.keyword.keymanagementserviceshort}} API.

### Retrieving a list of keys
{: #retrieve-keys-api}

For a high-level view, you can browse keys that are managed in your provisioned instance of {{site.data.keyword.keymanagementserviceshort}} by making a 
`GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. View general characteristics about your keys by running the following cURL command.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>' \
    -H 'correlation-id: <correlation_ID>'
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
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area 
        where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see 
        <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, 
        including the Bearer value, in the cURL request. 
        For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For 
        more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 2. Describes the variables that are needed to view keys with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `GET api/v2/keys` request returns a collection of keys that are available in your {{site.data.keyword.keymanagementserviceshort}} service instance.

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
    {:screen}

    By default, `GET api/v2/keys` returns your first 200 keys, but you can adjust this limit by using the `limit` parameter at query time. To learn more about `limit` 
    and `offset`, see [Retrieving a subset of keys](#retrieve-subset-keys-api).

    Not seeing the full list of keys? You might need to use `limit` and `offset` or check with your administrator to ensure you're assigned the correct level access to 
    keys in your instance. To learn more, see [Unable to view or list keys](/docs/key-protect?topic=key-protect-troubleshooting#unable-to-list-keys-api).
    {: tip}

### Retrieving a subset of keys
{: #retrieve-subset-keys-api}

By specifying the `limit` and `offset` parameters at query time, you can retrieve a subset of your keys, starting with the `offset` value that you specify.

For example, you might have 3000 total keys that are stored in your {{site.data.keyword.keymanagementserviceshort}} service instance, but you want to 
retrieve keys 200 - 300 when you make a `GET /keys` request.

You can use the following example request to retrieve a different set of keys.

  ```cURL
  curl -X GET \
  https://<region>.kms.cloud.ibm.com/api/v2/keys?offset=<offset>&limit=<limit> \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>' \
  ```
  {: codeblock}

  Replace the `limit` and `offset` variables in your request according to the following table.
  <table>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><p><varname>offset</varname></p></td>
      <td>
        <p>The number of keys to skip.</p>
        <p>For example, if you have 50 keys in your instance, and you want to list keys 26 - 50, use
            <code>../keys?offset=25</code>. You can also pair <code>offset</code> with <code>limit</code> to page through your available resources.</p>
      </td>
    </tr>
    <tr>
      <td><p><varname>limit</varname></p></td>
      <td>
        <p>The number of keys to retrieve.</p>
        <p>For example, if you have 100 keys in your instance, and you want to list only 10 keys, use
            <code>../keys?limit=10</code>. The maximum value for <code>limit</code> is 5000.</p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Table 2. Describes the <code>limit</code> and <code>offset</code> variables</caption>
  </table>

For usage notes, check out the following examples for setting your `limit` and `offset` query parameters.

<table>
  <tr>
    <th>URL</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>.../keys</code></td>
    <td>Lists all of your available resources, up to the first 200 keys.</td>
  </tr>
  <tr>
    <td><code>.../keys?limit=10</code></td>
    <td>Lists the first 10 keys.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=25&limit=50</code></td>
    <td>Lists keys 26 - 75.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=3000&limit=50</code></td>
    <td>Lists keys 3001 - 3050.</td>
  </tr>
  <caption style="caption-side:bottom;">Table 3. Provides usage notes for the limit and offset query parameters</caption>
</table>

Offset is the location of a particular key in a data set. The `offset` value is zero-based, which means that the 10th encryption key in a data set is at 
offset 9.
{: tip}

### Retrieving keys by state
{: #filter-keys-state-api}


By specifying the `state` parameter at query time, you can retrieve keys that are in the states that you specify.

For example, you might have keys in your service instance that are in the active, suspended, and destroyed states, but you only want to retrieve keys in the 
active state when you make a `GET /keys` request.

The state query parameter takes in a list of integers from 0 to 5 delimited by commas with no whitespace or trailing commas. Valid states are based on NIST 
SP 800-57.
For more information on key states, see [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).
{: note}

You can use the following example request to retrieve a different set of keys.

  ```cURL
  curl -X GET \
  https://<region>.kms.cloud.ibm.com/api/v2/keys?state=<state_integers> \
  -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <IAM_token>' \
  -H 'bluemix-instance: <instance_ID>' \
  ```
  {: codeblock}

  Replace the `state` variable in your request according to the following table.
  <table>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr> 
    <tr>
      <td><p><varname>state</varname></p></td>
      <td>
        <p>The states of the keys to be retrieved. States are integers and correspond to the Pre-activation = 0, Active = 1, Suspended = 2, Deactivated = 3, 
        and Destroyed = 5 values.</p>
        <p>For example, if you want to only list keys in the active state in your service instance, use
            <code>../keys?state=1</code>. You can also pair <code>state</code> with<code>offset</code> with <code>limit</code> to page through your 
            available resources.
            </p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Table 4. Describes the <code>state</code> variable.</caption>
  </table>

For usage notes, check out the following examples for setting your `state` query parameter.

<table>
  <tr>
    <th>URL</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>.../keys</code></td>
    <td>Lists all of your available resources, up to the first 200 keys.</td>
  </tr>
  <tr>
    <td><code>.../keys?state=5</code></td>
    <td>Lists keys in the deleted state.</td>
  </tr>
  <tr>
    <td><code>.../keys?state=2,3</code></td>
    <td>Lists keys in the suspended and deactivated state.</td>
  </tr>
  <caption style="caption-side:bottom;">Table 5. Provides usage notes for the stage query parameter.</caption>
</table>