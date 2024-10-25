---

copyright:
  years: 2017, 2024
lastupdated: "2024-10-25"

keywords: Key Protect availability, Key Protect disaster recovery

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

# High availability and disaster recovery
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} is a highly available, regional service with automatic features that help keep your applications secure and operational.
{: shortdesc}

The high availability of {{site.data.keyword.keymanagementserviceshort}} and the disaster recovery of data are guaranteed by {{site.data.keyword.cloud_notm}} and {{site.data.keyword.keymanagementserviceshort}}. Users are not required to perform any further actions. Note that "disaster recovery" does not include the ability to recover from user-initiated accidental deletions.

## Locations, tenancy, and availability
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional service.

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one of the supported [{{site.data.keyword.cloud_notm}} regions](/docs/key-protect?topic=key-protect-regions), which represent the geographic area where your {{site.data.keyword.keymanagementserviceshort}} requests are handled and
processed. Each {{site.data.keyword.cloud_notm}} region contains [multiple availability zones](https://www.ibm.com/blog/announcement/expansion-availability-zones-global-regions/){: external} to meet local access, low latency, and security requirements for the region.

There are three regions, `us-south` (located in Dallas, Texas, United States), `jp-tok` (located in Tokyo, Japan), and `eu-de` (located in Frankfurt, Germany) where {{site.data.keyword.keymanagementserviceshort}} has failover support into a separate region where data is replicated into standby infrastructure. The `us-south` region has data replicated into the `us-east` region (located in Washington DC, United States), the `jp-tok` region has data replicated into the `jp-osa` region (located in Osaka, Japan), and the `eu-de` region has data replicated into a datacenter in the Paris region. This means that if service in `us-south`, `jp-tok`, or `eu-de` is disrupted, requests to {{site.data.keyword.keymanagementserviceshort}} in those regions are routed to the region where the data has already been replicated. Note that the standby infrastructure in the region with failover support is completely separate from the {{site.data.keyword.keymanagementserviceshort}} service available in that region, and that failover only goes in one direction. Your data in `us-east`, for example, is not currently replicated to `us-south`.

Because your requests are routed automatically, you can continue to call your original endpoint. Initially, read-only operations are all that will be available. However, if there is reason to believe the disruption will last for an extended period, write operations will be enabled as soon as possible. Otherwise, read-only operations are all that will be available for up to six hours before write operations will be enabled.

{{site.data.keyword.keymanagementserviceshort}} will be adding cross-regional high availability to selected regions in different geographical locations.
{: tip}

As you plan your encryption strategy with {{site.data.keyword.cloud_notm}}, remember that provisioning {{site.data.keyword.keymanagementserviceshort}} in a region that is nearest to you is more likely to result in faster, more reliable connections when you interact with the {{site.data.keyword.keymanagementserviceshort}} APIs. Furthermore, regulatory requirements might influence which region or regions to use. Note that {{site.data.keyword.keymanagementserviceshort}} does not restrict using keys across regions and geographical locations. For example, a user might use keys from `us-south` to encrypt storage in Frankfurt. **Choose the region that is best for your use case overall**.

## Application-level high availability
{: #application-level-high-availability}

If you import a root key into {{site.data.keyword.keymanagementserviceshort}}, you are encouraged to maintain a secure backup of the key material so that you can restore the root key if it is accidentally deleted and the [30-day grace period to restore a deleted key](/docs/key-protect?topic=key-protect-delete-purge-keys) has elapsed. In other words, within 30 days, {{site.data.keyword.keymanagementserviceshort}} allows customers to restore a deleted key without having to use a backup.

Because two keys created using the same key material are effectively identical (both can unwrap any data encryption keys (DEKs) created by either root key), it is possible to securely backup your root keys by creating a duplicate key for every imported root key within your instance. Each duplicate key should be created in a {{site.data.keyword.keymanagementserviceshort}} region that is different from the original key, which means that the `key_id` and service instance ID of the keys will be different. Your application will therefore be able to use either key as long as it knows the relevant IDs and endpoints of both keys.

Note that every time a root key is rotated, new key material is added to the key, which creates a new version of the key. Therefore, make sure to use this updated key material when rotating your duplicated keys.

Applications that communicate over networks are subject to transient faults. You should design your application to interact with {{site.data.keyword.keymanagementserviceshort}} by using modern resiliency techniques, for example an exponential backoff that retries requests with exponentially increasing delays between each request.
{: tip}

### Example algorithm
{: #example-backoff-algorithm}

There are many approaches to implementing retries with exponential backoff
logic. Your approach will depend on your specific use case and the network
conditions surrounding your application. The following is an example
implementation of incremental retry delay.

```go
const maxRetries = 3
attempt := 1
delay := time.Second * 1
var ok bool
for !ok && attempt <= maxRetries {
    ok = makeRequest()
    if !ok {
        time.Sleep(delay)
        delay = delay * 2
        attempt += 1
    }
}
```
{: codeblock}

Once the maximum amount of retries has been reached and you have confirmed that
the errors your application is experiencing are due to
{{site.data.keyword.keymanagementserviceshort}}, open a
[support ticket](https://www.ibm.com/mysupport){: external}
with details regarding your request.
{: note}

## Disaster recovery
{: #dr}

{{site.data.keyword.keymanagementserviceshort}} follows {{site.data.keyword.cloud_notm}} requirements for [planning and recovering from disaster events](/docs/overview?topic=overview-zero-downtime#disaster-recovery){: external}.

Private endpoint settings, specifically the Internet Protocol (IP) address, may need to be manually updated during [Disaster recovery and business continuity actions](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).
{: important}

To find out more about the responsibilities that you and {{site.data.keyword.IBM_notm}} share in the maintenance of your keys and workloads, see [Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).
