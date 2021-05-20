---

copyright:
  years: 2021
lastupdated: "2021-01-21"

keywords: sync resources, sync registrations, internal, key registration, KYOK, BYOK

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}

# Sync associated resources
{: #sync-associated-resources}

You can initiate a manual data synchronization request between root keys 
and other cloud resources, such as Cloud Object Storage buckets or Cloud 
Databases deployments, by using the
{{site.data.keyword.keymanagementservicelong_notm}} API.
{: shortdesc}


When you perform an operation on a root key that is associated with other 
IBM cloud services, those IBM cloud services are notified of the key 
lifecycle event and are encouraged to respond accordingly. In the case that 
the cloud services do not respond to the key lifecycle notification, you can 
use the sync API to initiate of a renotification of the key lifecycle event 
to those associated cloud services. 

For example, you might delete a root key that has an association with Cloud 
Object Storage (COS). After waiting 4 hours for changes to take 
effect, you notice that you are still able to access the key's resources 
despite expecting to be blocked from accessing those resources. In this case, 
you should call the sync API to renotify COS of the deleted key lifecycle 
event so they can block access to the key's resources.

The sync API only initiates a request for synchronization. The IBM services 
associated with the key are responsible for managing all related associated 
resources and ensuring that the key state and key versions are up to date.
{: important}

## Syncing associated resources with the API
{: #sync-associated-resources-api}

You can renotify associated IBM cloud services of your 
{{site.data.keyword.keymanagementserviceshort}} root key's lifecycle event by
using the {{site.data.keyword.keymanagementserviceshort}} API.

You can initiate the renotification of a key lifecycle event by making a 
`POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/sync
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. Initiate a manual data synchronization request by running the
   following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/actions/sync" \
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
          <varname>key_ID</varname>
        </td>
        <td>
          <p>
            <strong>Required.</strong> The identifier for the root key that is
            associated with the cloud resources that you want to view.
          </p>
          <p>
            For more information, see
            [View Keys](/docs/key-protect?topic=key-protect-view-keys).
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

      <caption style="caption-side:bottom;">
        Table 3. Describes the variables that are needed to initiate a renotification of a key
        lifecycle event.
      </caption>
    </table>

    A successful `GET api/v2/keys/<key_ID>/actions/sync"` request returns an HTTP `204 No Content` 
    response, which indicates that the IBM cloud service that manages the associated resources of 
    the specified key has been notified.


    The sync API can only be called once per hour per key. If you send a request to this API and 
    the key has been synced within the past hour, the API will return a `409 Conflict` response.
    {: note}