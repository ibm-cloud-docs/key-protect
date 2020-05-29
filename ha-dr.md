---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-19"

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

{{site.data.keyword.keymanagementservicefull}} is a highly available, regional
service with automatic features that help keep your applications secure and
operational.
{: shortdesc}

Use this page to learn more about {{site.data.keyword.keymanagementserviceshort}}'s
availability and disaster recovery strategies.

## Locations, tenancy, and availability
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} is a multi-tenant, regional
service.

You can create {{site.data.keyword.keymanagementserviceshort}} resources in one
of the supported
[{{site.data.keyword.cloud_notm}} regions](/docs/key-protect?topic=key-protect-regions#regions),
which represent the geographic area where your
{{site.data.keyword.keymanagementserviceshort}} requests are handled and
processed. Each {{site.data.keyword.cloud_notm}} region contains
[multiple availability zones](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external}
to meet local access, low latency, and security requirements for the region.

As you plan your encryption at rest strategy with {{site.data.keyword.cloud_notm}},
keep in mind that provisioning {{site.data.keyword.keymanagementserviceshort}}
in a region that is nearest to you is more likely to result in faster, more
reliable connections when you interact with the {{site.data.keyword.keymanagementserviceshort}}
APIs.

Choose a specific region if the users, apps, or services that depend on a
{{site.data.keyword.keymanagementserviceshort}} resource are geographically
concentrated. Remember that users and services who are far away from the region
might experience higher latency.

Your encryption keys are confined to the region that you create them in.
{{site.data.keyword.keymanagementserviceshort}} does not copy or export
encryption keys to other regions.
{: note}

## Exponential Backoff
{: #exponential-backoff}

Apps or services that communicate over networks are subject to cloud services periodically being intermittently unavailable for more 
than a few seconds for any reason. You want to design your applications to retry connections when errors are caused by a temporary loss 
in connectivity to {{site.data.keyword.keymanagementserviceshort}}. Exponential backoff is a technique that retries requests 
exponentially, with increasing delays between each request. Using exponential backoff logic for your retry mechanism is beneficial 
because it helps to reduce the workload of the service, which aids in the service's recovery.

### Example Algorithm
{: #example-backoff-algorithm}

There are many approaches to implementing retries with exponential backoff logic. Your approach will depend on your specific use case 
and the network conditions surrounding your application. The following is an example implementation of incremental retry delay.

```
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

Once the maximum amount of retries has been reached, open an IBM support ticket with details regarding your request. For more 
information about opening an IBM support ticket, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
{:note}

## Disaster recovery
{: #dr}

{{site.data.keyword.keymanagementserviceshort}} follows {{site.data.keyword.cloud_notm}}
requirements for
[planning and recovering from disaster events](/docs/overview?topic=overview-zero-downtime#disaster-recovery).

To find out more about the responsibilities that you and IBM share for disaster
recovery, see
[Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).
