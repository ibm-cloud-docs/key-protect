---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Setting a rotation policy
{: #set-rotation-policy}

You can set an automatic rotation policy for a root key by using {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

When you set an automatic rotation policy for a root key, you shorten the lifetime of the key at regular intervals, and you limit the amount of information that is protected by that key.

You can create a rotation policy only for root keys that are generated in {{site.data.keyword.keymanagementserviceshort}}. If you imported the root key initially, you must provide new base64 encoded key material to rotate the key. For more information, see [Rotating root keys on-demand](/docs/key-protect?topic=key-protect-rotate-keys#rotate-keys).
{: note}

Want to learn more about your key rotation options in {{site.data.keyword.keymanagementserviceshort}}? Check out [Comparing your key rotation options](/docs/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options) for more information.
{: tip}

## Managing rotation polices in the console
{: #manage-policies-gui}

If you prefer to manage policies for your root keys by using a graphical interface, you can use the {{site.data.keyword.cloud_notm}} console.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
2. Go to **Menu** &gt; **Resource List** to view a list of your resources.
3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.
4. On the application details page, use the **Keys** table to browse the keys in your service.
5. Click the ⋯ icon to open a list of options for a specific key.
6. From the options menu, click **Manage policy** to manage the rotation policy for the key.
7. From the list of rotation options, select a frequency of rotation in months.

    If your key has an existing rotation policy, the interface displays the key's existing rotation period.

8. Click **Create policy** to set the policy for the key.

When it's time to rotate the key based on the rotation interval that you specify, {{site.data.keyword.keymanagementserviceshort}} automatically replaces the root key with new key material.

## Managing rotation policies with the API
{: #manage-rotation-policies-api}

### Viewing a rotation policy
{: #view-rotation-policy-api}

For a high-level view, you can browse the rotation policies that are associated with a root key by making a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the rotation policy for a specified key by running the following cURL command.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the root key that has an existing rotation policy.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
        <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to create a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `GET api/v2/keys/{id}/policies` response returns policy details that are associated with your key. The following JSON object shows an example response for a root key that has an existing rotation policy.

    ```json
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    The `interval_month` value indicates the key rotation frequency in months.

### Creating a rotation policy
{: #create-rotation-policy-api}

Create a rotation policy for your root key by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Create a rotation policy for a specified key by running the following cURL command.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
       }
      ]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the root key that you want to create a rotation policy for.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>rotation_interval</varname></td>
        <td><strong>Required.</strong> An integer value that determines the key rotation interval time in months. The minimum is <code>1</code> and the maximum is <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to create a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `PUT api/v2/keys/{id}/policies` response returns policy details that are associated with your key. The following JSON object shows an example response for a root key that has an existing rotation policy.

    ```json
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### Updating a rotation policy
{: #update-rotation-policy-api}

Update an existing policy for a root key by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Retrieve your service and authentication credentials](/docs/key-protect?topic=key-protect-set-up-api).

2. Replace the rotation policy for a specified key by running the following cURL command.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
       }
      ]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the root key that you want to replace a rotation policy for.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>Required.</strong> An integer value that determines the key rotation interval time in months. The minimum is <code>1</code> and the maximum is <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to create a rotation policy with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `PUT api/v2/keys/{id}/policies` response returns updated policy details that are associated with your key. The following JSON object shows an example response for a root key with an updated rotation policy.

    ```json
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
        "resources": [
        {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    The `interval_month` and `updatedat` values are updated in the policy details for the key. If a different user updates a policy for a key that you created initially, the `updatedby` value also changes to show the identifier for the person who sent the request.
