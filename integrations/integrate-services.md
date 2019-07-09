---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect integration, integrate service with Key Protect

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

# Integrating services
{: #integrate-services}

{{site.data.keyword.keymanagementservicefull}} integrates with data and storage solutions to help you bring and manage your own encryption in the cloud.
{: shortdesc}

[After you create an instance of the service](/docs/services/key-protect?topic=key-protect-provision), you can integrate {{site.data.keyword.keymanagementserviceshort}} with the following supported services:

| Service | Description |
| --- | --- |
| {{site.data.keyword.cos_full_notm}} | Add [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption) to your storage buckets by using {{site.data.keyword.keymanagementserviceshort}}. Use root keys that you manage in {{site.data.keyword.keymanagementserviceshort}} to protect the data encryption keys that encrypt your data at rest. To learn more, check out [Integrating with {{site.data.keyword.cos_full_notm}}](/docs/services/key-protect?topic=key-protect-integrate-cos).|
| {{site.data.keyword.databases-for-postgresql_full_notm}} | Protect your databases by associating root keys with your {{site.data.keyword.databases-for-postgresql}} deployment. To learn more, check out the [{{site.data.keyword.databases-for-postgresql}} documentation](/docs/services/databases-for-postgresql?topic=cloud-databases-key-protect).|
| {{site.data.keyword.cloudant_short_notm}} for {{site.data.keyword.cloud_notm}} ({{site.data.keyword.cloud_notm}} Dedicated) | Strengthen your encryption at rest strategy by associating root keys with your {{site.data.keyword.cloudant_short_notm}} Dedicated Hardware instance. To learn more, check out the [{{site.data.keyword.cloudant_short_notm}} documentation](/docs/services/Cloudant/offerings?topic=cloudant-security#secure-access-control). |
| {{site.data.keyword.containerlong_notm}} | Use [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption) to protect secrets in your {{site.data.keyword.containershort_notm}} cluster. To learn more, check out [Encrypting Kubernetes secrets by using {{site.data.keyword.keymanagementserviceshort}} ](/docs/containers?topic=containers-encryption#keyprotect).|
{: caption="Table 1. Describes the integrations that are available with {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

## Understanding your integration 
{: #understand-integration}

When you integrate a supported service with {{site.data.keyword.keymanagementserviceshort}}, you enable [envelope encryption](/docs/services/key-protect?topic=key-protect-envelope-encryption) for that service. This integration allows you to use a root key that you store in {{site.data.keyword.keymanagementserviceshort}} to wrap the data encryption keys that encrypt your data at rest. 

For example, you can create a root key, manage the key in {{site.data.keyword.keymanagementserviceshort}}, and use the root key to protect the data that is stored across different cloud services.

![The diagram shows a contextual view of your {{site.data.keyword.keymanagementserviceshort}} integration.](../images/kp-integrations.svg)

### {{site.data.keyword.keymanagementserviceshort}} API methods
{: #envelope-encryption-api-methods}

Behind the scenes, the {{site.data.keyword.keymanagementserviceshort}} API drives the envelope encryption process.  

The following table lists the API methods that add or remove envelope encryption on a resource.

| Method | Description |
| --- | --- |
| `POST /keys/{root_key_ID}?action=wrap` | [Wrap (encrypt) a data encryption key](/docs/services/key-protect?topic=key-protect-wrap-keys) |
| `POST /keys/{root_key_ID}?action=unwrap` | [Unwrap (decrypt) a data encryption key](/docs/services/key-protect?topic=key-protect-unwrap-keys) |
{: caption="Table 2. Describes the {{site.data.keyword.keymanagementserviceshort}} API methods" caption-side="top"}

To find out more about programmatically managing your keys in {{site.data.keyword.keymanagementserviceshort}}, check out the [{{site.data.keyword.keymanagementserviceshort}} API reference doc](https://{DomainName}/apidocs/key-protect){: external}.
{: tip}

## Integrating a supported service
{: #grant-access}

To add an integration, create an authorization between services by using the {{site.data.keyword.iamlong}} dashboard. Authorizations enable service to service access policies, so you can associate a resource in your cloud data service with a [root key](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types) that you manage in {{site.data.keyword.keymanagementserviceshort}}.

Be sure to provision both services in the same region before you create an authorization. To learn more about service authorizations, see [Granting access between services](/docs/iam?topic=iam-serviceauth){: external}.
{: note}

When you're ready to integrate a service, use the following steps to create an authorization:

1. From the menu bar, click **Manage** &gt; **Security** &gt; **Access (IAM)**, and select **Authorizations**. 
2. Click **Create**.
3. Select a source and target service for the authorization.
 
  For **Source service**, select the cloud data service that you want to integrate with {{site.data.keyword.keymanagementserviceshort}}. For **Target service**, select **{{site.data.keyword.keymanagementservicelong_notm}}**.

5. Enable the **Reader** role.

    With _Reader_ permissions, your source service can browse the root keys that are provisioned in the specified instance of {{site.data.keyword.keymanagementserviceshort}}.

6. Click **Authorize**.

## What's next
{: #integration-next-steps}

Add advanced encryption to your cloud resources by creating a root key in {{site.data.keyword.keymanagementserviceshort}}. Add a new resource to a supported cloud data service, and then select the root key that you want to use for advanced encryption.

- To find out more about creating root keys with the {{site.data.keyword.keymanagementserviceshort}} service, see [Creating root keys](/docs/services/key-protect?topic=key-protect-create-root-keys).
- To find out more about bringing your own root keys to the {{site.data.keyword.keymanagementserviceshort}} service, see [Importing root keys](/docs/services/key-protect?topic=key-protect-import-root-keys).


