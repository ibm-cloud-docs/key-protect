---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-22"

keywords: instance settings, service settings, operational metrics, metrics

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

# Monitoring metrics
{: #manage-monitor-metrics}

After you set up your {{site.data.keyword.keymanagementservicelong}} service
instance, you can manage monitoring metrics by using the service API.
{: shortdesc}

## Metrics settings
{: #manage-metrics-policy}

Metrics for {{site.data.keyword.keymanagementserviceshort}} service instances is
an extra policy that allows you to receive the operational metrics for your
{{site.data.keyword.keymanagementserviceshort}} instance. When you enable this
policy, {{site.data.keyword.mon_short}} can be used to monitor any operations
that are performed on the resources in your
{{site.data.keyword.keymanagementserviceshort}} instance.

Enabling metrics is only available through the
{{site.data.keyword.keymanagementserviceshort}} API. To find out more about
accessing the {{site.data.keyword.keymanagementserviceshort}} APIs, check out
[Setting up the API](/docs/key-protect?topic=key-protect-set-up-api).
{: preview}

Before you enable operational metrics for your
{{site.data.keyword.keymanagementserviceshort}} instance, keep in mind the
following considerations:

- **When you enable metrics for your {{site.data.keyword.keymanagementserviceshort}} instance, metrics are only reported after the time that the policy is enabled.**
  Once your metrics policy is enabled, you will see operational metrics for all
  API requests that occur in your instance after the the policy is activated.
  You will not be able to view any metrics prior to the time that the policy is
  enabled.

- **You will need to provision a {{site.data.keyword.mon_short}} instance for your policy in order to see the metrics.**
  You will need to
  [provision a {{site.data.keyword.mon_short}} instance](/docs/monitoring?topic=monitoring-provision){: external}
  that is located in the same region as the
  {{site.data.keyword.keymanagementserviceshort}} instance that you would like
  to receive operational metrics for. Once you provision the
  {{site.data.keyword.mon_short}} instance, you will need to
  [enable platform metrics](/docs/key-protect?topic=key-protect-operational-metrics#configure-monitor).

### Enabling metrics for your {{site.data.keyword.keymanagementserviceshort}} instance with the Console
{: #enable-metrics-instance-policy-ui}

After creating a {{site.data.keyword.keymanagementserviceshort}} instance,
provisioning a {{site.data.keyword.mon_short}}, and enabling platform metrics,
complete the following steps to enable a metrics policy:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your
   provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Instance policies** page, click the **Enable** button
    in the metrics policy section.

### Enabling metrics for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #enable-metrics-instance-policy-api}

As an instance manager, enable a metrics policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=metrics
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To enable metrics policies, you must be assigned a
    _Manager_ IAM access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM
    roles map to {{site.data.keyword.keymanagementserviceshort}} service
    actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Enable a metrics policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following `curl` command.

   ```sh
   curl -X  PUT \
   "https://<region>.kms.test.cloud.ibm.com/api/v2/instance/policies?policy=metrics" \
   -H "accept: application/vnd.ibm.kms.policy+json" \
   -H "authorization: Bearer <IAM_token>" \
   -H "bluemix-instance: <instance_ID>" \
   -H "content-type: application/vnd.ibm.kms.policy+json" \
   -d '{
           "metadata": {
               "collectionType": "application/vnd.ibm.kms.policy+json",
               "collectionTotal": 1
           },
           "resources": [
               {
                   "policy_type": "metrics",
                   "policy_data": {
                       "enabled": false
                   }
               }
           ]
     }'
   ```
   {: codeblock}

   Replace the variables in the example request according to the following
   table.

   |Variable|Description|
   |--- |--- |
   |region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
   |IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
   |instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
   {: caption="Table 1. Describes the variables that are needed to enable a metrics policy." caption-side="top"}

   A successful request returns an HTTP `204 No Content` response, which
   indicates that your {{site.data.keyword.keymanagementserviceshort}} instance
   is now enabled for reporting operational metrics.

   This new policy only reports on operations that occur after the policy is
   enabled.
   {: note}

3. Optional: Verify that the metrics policy was created by browsing
   the policies that are available for your
   {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=metrics" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

### Disabling metrics for your {{site.data.keyword.keymanagementserviceshort}} instance with the API
{: #disable-metrics-api}

As an instance manager, disable an existing metrics policy for a
{{site.data.keyword.keymanagementserviceshort}} instance by making a
`PUT` call to the following endpoint.

```plaintext
https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=metrics
```
{: codeblock}

1. [Retrieve your authentication credentials to work with the API](/docs/key-protect?topic=key-protect-set-up-api).

    To disable metrics policies, you must be assigned a
    _Manager_ access policy for your
    {{site.data.keyword.keymanagementserviceshort}} instance. To learn how IAM
    roles map to {{site.data.keyword.keymanagementserviceshort}} service
    actions, check out
    [Service access roles](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: note}

2. Disable an existing metrics policy for your
   {{site.data.keyword.keymanagementserviceshort}} instance by running the
   following `curl` command.

   ```sh
    $ curl -X PUT \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=metrics" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>" \
        -H "content-type: application/vnd.ibm.kms.policy+json" \
        -d '{
                "metadata": {
                    "collectionType": "application/vnd.ibm.kms.policy+json",
                    "collectionTotal": 1
                },
                "resources": [
                    {
                        "type": "application/vnd.ibm.kms.policy+json",
                        "metrics": {
                            "enabled": false
                        }
                    }
                ]
            }'
   ```
   {: codeblock}

   Replace the variables in the example request according to the following
   table.
   
|Variable|Description|
|--- |--- |
|region|**Required**. The region abbreviation, such as `us-south` or `eu-gb`, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} instance resides.<br><br>For more information, see [Regional service endpoints](/docs/key-protect?topic=key-protect-regions#service-endpoints).|
|IAM_token|**Required**. Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the IAM token, including the Bearer value, in the curl request.<br><br>For more information, see [Retrieving an access token](/docs/key-protect?topic=key-protect-retrieve-access-token).|
|instance_ID|**Required**. The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance.<br><br>For more information, see [Retrieving an instance ID](/docs/key-protect?topic=key-protect-retrieve-instance-ID).|
{: caption="Table 2. Describes the variables that are needed to enable metrics policies." caption-side="top"}

   A successful request returns an HTTP `204 No Content` response, which
   indicates that the metrics policy was updated for your service
   instance.

Optional: Verify that the metrics policy was updated by browsing
   the policies that are available for your
   {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    $ curl -X GET \
        "https://<region>.kms.cloud.ibm.com/api/v2/instance/policies?policy=metrics" \
        -H "accept: application/vnd.ibm.kms.policy+json" \
        -H "authorization: Bearer <IAM_token>" \
        -H "bluemix-instance: <instance_ID>"
    ```
    {: codeblock}

## What's next
{: #monitor-metrics-next-steps}

- To find out more about configuring your
  {{site.data.keyword.keymanagementserviceshort}} instance with
  {{site.data.keyword.mon_short}}, check out
  [Monitoring Operational Metrics](/docs/key-protect?topic=key-protect-operational-metrics).
