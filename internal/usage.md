---

copyright:
  years: 2017, 2019
lastupdated: "2019-10-07"

keywords: Key Protect integration, integrate COS with Key Protect

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

# Usage information
{: #usage}

Review internal-only information about the
{{site.data.keyword.keymanagementservicelong}} service.
{: shortdesc}

- To get help with adoption, give feedback about the service or its
    documentation, or chat with our development team, post your questions in the
    [`#key-protect` Slack channel](https://ibm-cloudplatform.slack.com/archives/key-protect){: external}.

- Need to onboard your service with
    {{site.data.keyword.keymanagementserviceshort}}? Check out our
    [getting started guide](/docs/key-protect?topic=key-protect-onboard-service).
    For information about integrating your service as a KMS provider, see
    [Working with KMS providers](/docs/key-protect?topic=key-protect-kms-providers).

- To raise an issue about the {{site.data.keyword.keymanagementserviceshort}}
    service, open a new issue in our
    [{{site.data.keyword.keymanagementserviceshort}} GitHub repository](https://github.ibm.com/kms/customer-issues/issues/new/choose){: external}.
    In the GitHub issue, be sure to include any additional information, such as
    the `correlation-id` from the response header, a timestamp, or your REST API
    call structure.

## Architecture
{: #usage-arch}

- For information about the {{site.data.keyword.keymanagementserviceshort}}
    architecture, see the
    [{{site.data.keyword.keymanagementserviceshort}} architecture GitHub repository](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/){: external}.

- For information about adoption patterns for
    {{site.data.keyword.keymanagementserviceshort}}, see
    [Adoption Patterns](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/architecture/adoption/){: external}.

## Environments and pricing
{: #usage-environments}

### Which {{site.data.keyword.keymanagementserviceshort}} environment should I use to run my test and production workloads?
{: #usage-which-env}

Internal teams can target the following endpoint for running test workloads
against our stage environment.

```plaintext
https://qa.us-south.kms.test.cloud.ibm.com
```
{: codeblock}

Keep in mind that we do not provide the same SLAs in stage as we do in
production. Our staging environment is best effort, and it may become
unavailable or unstable at any point in time. To consume features that are
tested thoroughly and approved for external use, target our production
environment.
{: note}

### How does pricing work for {{site.data.keyword.keymanagementserviceshort}}?
{: #usage-pricing}

{{site.data.keyword.keymanagementserviceshort}} offers a
[graduated tier plan](/catalog/services/key-protect){: external}
with no-charge pricing for users requiring 20 or fewer keys. You can have 20
free keys per {{site.data.keyword.cloud_notm}} account. If your team requires
multiple instances of {{site.data.keyword.keymanagementserviceshort}}, BSS adds
your active keys across all instances within the account and then applies
pricing. Internal teams receive the
[standard {{site.data.keyword.cloud_notm}} PaaS discount](https://pages.github.ibm.com/Bluemix/chargeback-faq/faq.html){: external}.

#### What is an active key?
{: #usage-active-key}

When you import encryption keys into
{{site.data.keyword.keymanagementserviceshort}}, or when you use
{{site.data.keyword.keymanagementserviceshort}} to generate keys from its HSMs,
those keys become _Active_ keys. Pricing is based on all active keys within an
{{site.data.keyword.cloud_notm}} account.

#### How should I group and manage my keys?
{: #usage-manage-keys}

From a pricing standpoint, the best way to use
{{site.data.keyword.keymanagementserviceshort}} is to create a limited number of
root keys, and then use those root keys to encrypt the data encryption keys
(DEKs) that are created by an external app or cloud data service.
{{site.data.keyword.keymanagementserviceshort}} does not charge for the DEKs. To
find out more, see
[Adoption Patterns](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/architecture/adoption/){: external}

## Regions and endpoints
{: #usage-endpoints}

You can structure your API calls by using the following service API endpoints.

### Stage endpoints
{: #usage-stage-endpoints}

| Location | Public endpoints (stage)             | Private endpoints (stage)                    |
| -------- | ------------------------------------ | -------------------------------------------- |
| Dallas   | `qa.us-south.kms.test.cloud.ibm.com` | `private.qa.us-south.kms.test.cloud.ibm.com` |

### Production endpoints
{: #usage-prod-endpoints}

| Location      | Public endpoints (prod)      | Private endpoints (prod)             |
| ------------- | ---------------------------- | ------------------------------------ |
| Dallas        | `us-south.kms.cloud.ibm.com` | `private.us-south.kms.cloud.ibm.com` |
| Washington DC | `us-east.kms.cloud.ibm.com`  | `private.us-east.kms.cloud.ibm.com`  |
| London        | `eu-gb.kms.cloud.ibm.com`    | `private.eu-gb.kms.cloud.ibm.com`    |
| Frankfurt     | `eu-de.kms.cloud.ibm.com`    | `private.eu-de.kms.cloud.ibm.com`    |
| Sydney        | `au-syd.kms.cloud.ibm.com`   | `private.au-syd.kms.cloud.ibm.com`   |
| Tokyo         | `jp-tok.kms.cloud.ibm.com`   | `private.jp-tok.kms.cloud.ibm.com`   |

If you're using a `bluemix.net` API endpoint to target operations for the
service, you're using a legacy instance of
{{site.data.keyword.keymanagementserviceshort}} that is based on Cloud Foundry.

You can check to see whether you're using a
legacy instance by navigating to your resource list from the
[{{site.data.keyword.cloud_notm}} console](/login/){: external}.

If your {{site.data.keyword.keymanagementserviceshort}} instance is
listed in the **Cloud Foundry Services** section of the
{{site.data.keyword.cloud_notm}} resource list, or if you're using a
`bluemix.net` API endpoint to target operations for
the service, you're using a legacy instance of the
{{site.data.keyword.keymanagementserviceshort}}.

After 15 May 2019, the legacy endpoint will no longer be accessible. Migrate
your keys into a new {{site.data.keyword.keymanagementserviceshort}}
instance link to avoid losing access to any associated data.
{: important}

## Available functionality
{: #usage-functionality}

The following capabilities are available for
{{site.data.keyword.keymanagementserviceshort}}.

| Functionality                                                                           | Availability |
| --------------------------------------------------------------------------------------- | ------------ |
| [Create root keys](/docs/key-protect?topic=key-protect-create-root-keys)                | Available    |
| [Create standard keys](/docs/key-protect?topic=key-protect-create-standard-keys)        | Available    |
| [Import root keys](/docs/key-protect?topic=key-protect-import-root-keys)                | Available    |
| [Import standard keys](/docs/key-protect?topic=key-protect-import-standard-keys)        | Available    |
| [Retrieve a list of keys](/docs/key-protect?topic=key-protect-view-keys)                | Available    |
| [Retrieve a key by ID](/docs/key-protect?topic=key-protect-view-keys#retrieve-keys-api) | Available    |
| [Wrap (encrypt) keys](/docs/key-protect?topic=key-protect-wrap-keys)                    | Available    |
| [Unwrap (decrypt) keys](/docs/key-protect?topic=key-protect-unwrap-keys)                | Available    |
| [Rotate keys on-demand](/docs/key-protect?topic=key-protect-rotate-keys)                | Available    |
| [Set a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy)        | Available    |
| [Delete keys](/docs/key-protect?topic=key-protect-delete-keys)                          | Available    |


