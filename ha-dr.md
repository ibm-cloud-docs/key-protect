---

copyright:
  years: 2017, 2020
lastupdated: "2020-10-08"

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

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional
service.

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one
of the supported
[{{site.data.keyword.cloud_notm}} regions](/docs/key-protect?topic=key-protect-regions),
which represent the geographic area where your
{{site.data.keyword.keymanagementserviceshort}} requests are handled and
processed. Each {{site.data.keyword.cloud_notm}} region contains
[multiple availability zones](https://www.ibm.com/cloud/blog/announcements/expansion-availability-zones-global-regions){: external}
to meet local access, low latency, and security requirements for the region.

As you plan your encryption at rest strategy with
{{site.data.keyword.cloud_notm}}, keep in mind that provisioning
{{site.data.keyword.keymanagementserviceshort}} in a region that is nearest to
you is more likely to result in faster, more reliable connections when you
interact with the {{site.data.keyword.keymanagementserviceshort}} APIs.

Choose a specific region if the users, apps, or services that depend on a
{{site.data.keyword.keymanagementserviceshort}} resource are geographically
concentrated. Remember that users and services who are far away from the region
might experience higher latency.

Your encryption keys are confined to the region that you create them in.
{{site.data.keyword.keymanagementserviceshort}} does not copy or export
encryption keys to other regions.
{: note}

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
[support ticket](https://github.ibm.com/kms/customer-issues){: external}
with details regarding your request.
{: note}

## Disaster recovery
{: #dr}

{{site.data.keyword.keymanagementserviceshort}} follows {{site.data.keyword.cloud_notm}} requirements for [planning and recovering from disaster events](/docs/overview?topic=overview-zero-downtime#disaster-recovery){: external}.

To find out more about the responsibilities that you and {{site.data.keyword.IBM_notm}} share in the maintenance of your keys and workloads, see [Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).
