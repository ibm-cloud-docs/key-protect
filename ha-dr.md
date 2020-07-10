---

copyright:
  years: 2017, 2020
lastupdated: "2020-05-29"

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
[{{site.data.keyword.cloud_notm}} regions](/docs/key-protect?topic=key-protect-regions),
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

## Application-level High-Availability
{: #application-level-high-availability}

Applications that communicate over networks are subject to transient faults. You should design your application to interact with Key Protect by using modern
resiliency techniques, such as Exponential backoff. [Exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff){: external} is a technique that retries
requests exponentially, with increasing delays between each request.

### Example Algorithm
{: #example-backoff-algorithm}

There are many approaches to implementing retries with exponential backoff logic. Your approach will depend on your specific use case and the network conditions
surrounding your application. The following is an example implementation of incremental retry delay.

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

Once the maximum amount of retries has been reached and you have confirmed that
the errors your application is experiencing are due to
{{site.data.keyword.keymanagementserviceshort}}, open a
[support ticket](https://github.ibm.com/kms/customer-issues){: external}
with details regarding your request.
{: note}

## Disaster recovery
{: #dr}

{{site.data.keyword.keymanagementserviceshort}} follows {{site.data.keyword.cloud_notm}}
requirements for
[planning and recovering from disaster events](/docs/overview?topic=overview-zero-downtime#disaster-recovery){: external}.

To find out more about the responsibilities that you and IBM share for disaster
recovery, see
[Understanding your responsibilities with using {{site.data.keyword.keymanagementserviceshort}}](/docs/key-protect?topic=key-protect-shared-responsibilities#disaster-recovery).

