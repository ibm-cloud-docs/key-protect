---

copyright:
  years: 2023
lastupdated: "2023-07-05"

keywords: synchronize resources, sync registrations, BYOK

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
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:ui: .ph data-hd-interface='ui'}

# Sync associated resources
{: #sync-associated-resources}

You can initiate a manual data synchronization request between root keys and other cloud resources, such as {{site.data.keyword.cos_full_notm}} buckets or deployments of {{site.data.keyword.databases-for}}, by using the [{{site.data.keyword.keymanagementservicelong}} API](/apidocs/key-protect), [{{site.data.keyword.cloud_notm}} console](/login), or the  [{{site.data.keyword.keymanagementserviceshort}} CLI plugin](/docs/key-protect?topic=key-protect-key-protect-cli-reference).
{: shortdesc}


When you perform a key lifecycle action (for example `rotate`, `restore`,
`disable`, `enable`, `delete`) on a root key that is associated with other
{{site.data.keyword.cloud_notm}} services, those {{site.data.keyword.cloud_notm}} services are notified of the key
lifecycle event and are encouraged to respond accordingly. In the case that
the cloud services do not respond to the key lifecycle notification, you can
use the sync API to initiate of a renotification of the key lifecycle event
to those associated cloud services.

It can take up to four hours for services to respond to events and sync requests.
{: note}

For example, you might delete a root key that has an association with {{site.data.keyword.cos_full_notm}} (COS). After waiting 4 hours for changes to take
effect, you notice that you are still able to access the key's resources
despite expecting to be blocked from accessing those resources. In this case,
you should call the sync API to notify COS of the deleted key lifecycle
event so they can block access to the key's resources.

The sync API only initiates a request for synchronization. The {{site.data.keyword.cloud_notm}} services
associated with the key are responsible for managing all related associated
resources and ensuring that the key state and key versions are up to date.
{: important}

## Syncing associated resources with the {{site.data.keyword.keymanagementserviceshort}} console
{: #sync-associated-resources-ui}
{: ui}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
    provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. Click on the **Associated Resources** tab to see the list of resources.

5. Click on the **Manually sync** resource icon (e.g. ![sync icon](../images/resource-icon.svg)).

6. In the **Manually sync associated resource** dialog box, click on the button labeled, **Manually sync** to perform the synchronization.

## Syncing associated resources with the {{site.data.keyword.keymanagementserviceshort}} CLI plugin
{: #sync-associated-resources-cli}
{: cli}

This example shows how to synchronize a key and show the results. Before getting started, make sure to complete the [required configuration](/docs/key-protect?topic=key-protect-set-up-cli) for the {{site.data.keyword.keymanagementserviceshort}} CLI plugin.

```sh
# synchronize the associated resources for a given key
$ ibmcloud kp key sync 94c06f9c-a07a-4961-8548-553cf7431f18

Synchronizing key...
OK
Key's associated resources are synchronized successfully
```
{: screen}

### Required parameters
{: #kp-key-sync-required}

* **`keyID_or_alias`**

   The ID or alias of the key that you want to sync.

* **`-i, --instance-id`**

   The {{site.data.keyword.cloud_notm}} instance ID that identifies your {{site.data.keyword.keymanagementserviceshort}} instance.

   You can set an environment variable instead of specifying `-i` with the following command: **`$ export KP_INSTANCE_ID=<INSTANCE_ID>`**.

### Optional parameters
{: #kp-key-sync-optional}

* **`-o, --output`**

   Set the CLI output format. By default, all commands print in table format. To change the output format to JSON, use `--output json`.

* **`--key-ring`**

   A unique, human readable name for the key-ring. This is required if the user doesn't have permissions on the default key ring.



## Syncing associated resources with the API
{: #sync-associated-resources-api}
{: api}

You can notify associated IBM cloud services of your
{{site.data.keyword.keymanagementserviceshort}} root key's lifecycle event by
using the {{site.data.keyword.keymanagementserviceshort}} API.

You can initiate the renotification of a key lifecycle event by making a
`POST` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/sync
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

2. Initiate a manual data synchronization request by running the
    following `curl` command.

    ```sh
    $ curl -X POST \
        "https://<region>.kms.cloud.ibm.com/api/v2/keys/<keyID_or_alias>/actions/sync" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

    Replace the variables in the example request according to the following
    table.

|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south or eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|keyID_or_alias|**Required**. The identifier or alias for the root key that is associated with the cloud resources that you want to view.<br><br>For more information, see [View Keys](/docs/key-protect?topic=key-protect-view-keys).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 1. Describes the variables that are needed to initiate a re-notification of a key lifecycle event" caption-side="top"}


A successful `GET api/v2/keys/<keyID_or_alias>/actions/sync` request returns an HTTP `204 No Content`
response, which indicates that the IBM cloud service that is associated with the specified key
has been notified.

## Synchronization considerations
{: #kp-key-sync-considerations}

The sync API can only be initialized if it has been longer than an hour since the last
notification to the associated cloud services of the key. If you send a request to this API and
the key has been synced or a key lifecycle action has been taken within the past hour,
the API will return a `409 Conflict` response.
{: note}

You can [view associated resources](/docs/key-protect?topic=key-protect-view-protected-resources) to determine which services are relevant to synchronization. 
