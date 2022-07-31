---

copyright:
  years: 2022
lastupdated: "2022-07-31"

keywords: pricing plan, billing, cost

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
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}

# Pricing
{: #pricing-plan}
{: ui}
{: api}
{: cli}

{{site.data.keyword.keymanagementserviceshort}} is switching from a tiered pricing plan, in which the cost of a key depends on how many keys you have deployed, to a value-based pricing model that counts the total number of **key versions** in your account, whether keys are created or imported, root or standard. In this plan the first five key versions are free (per account), after which the price is USD1 per key version per month. Only non-deleted keys, which include all active and disabled keys, are counted for pricing purposes. This pricing model covers the value {{site.data.keyword.keymanagementserviceshort}} provides by managing all versions of the key material so they can be used to decrypt older ciphertexts.
{: shortdesc}

Creating or importing a key initiates the first **version** of that key. As a result, a newly created or imported key has one version. Each time a key is rotated, a new version is created. A key that has been rotated 10 times, then, has 11 versions (10 versions created by rotations plus the initial version). To discover how many versions you have of each non-deleted key, check out [How many key versions do you have?](#pricing-plan-how-many-keys). For more information about rotating keys, check out [Rotating your root keys](/docs/key-protect?topic=key-protect-key-rotation). Note that only root keys can be rotated. For the purpose of {{site.data.keyword.keymanagementserviceshort}} pricing, the [data encryption keys (DEKs)](/docs/key-protect?topic=key-protect-envelope-encryption) used by services or applications to encrypt the data are not counted as key versions in terms of pricing.

While {{site.data.keyword.keymanagementserviceshort}} does not charge a monthly fee for the maintenance of deployed instances, it does charge for the usage of its service through keys that are created, imported, rotated and otherwise managed. Note that "non-deleted" keys refers to any key that has not been moved into the `Destroyed` state. `Deactivated` and `Suspended` keys have not been deleted, and still count as non-deleted versions. For more information about key states, check out [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).

You do not need to perform any migration steps to switch to the new pricing plan.
{: important}

## Benefits of the new pricing model
{: #pricing-plan-benefits}
{: ui}
{: api}
{: cli}

* **Simplicity**: it is easy to understand, calculate, and plan for your costs. Use the UI to discover [how many versions of each key you have](#pricing-plan-how-many-keys).

* **No membership fees or minimum fees**: allows you invest directly in your keys and only pay for keys and key versions you create. No additional charges are assessed for functions, API calls, environments, components, or tiering.

* **First five key versions are free**: take {{site.data.keyword.keymanagementserviceshort}} for a test drive to learn how it fits the needs of your use case.

## Pricing scenarios
{: #pricing-plan-scenarios}

* **Scenario:** You have one non-deleted key that has been rotated 19 times, which means it has 20 total versions.
  - **Pricing:** Your charge is $15USD per month (the first five keys are free).

* **Scenario:** You have one non-deleted key that has been rotated four times, meaning the key has five total versions.
  - **Pricing:** Your charge is $0 per month.

* **Scenario:** You have one non-deleted key with 20 total versions, another key with three versions, and a key in the `Suspended` state with two versions.
  - **Pricing:** Your charge is $20 per month.

You are charged for all versions of all non-deleted keys that exist in your instances during the billing period (minus the five free key versions). This includes keys you create (or import) and delete before the month is over. If for example you create and delete a key the same day, you are charged $1 that month for the key (if you have more than five key versions in your instances). However, you will not be charged the following month for that key if it remains deleted. If you restore the key at any point, you will be charged for all of the versions of that key for that month. For more information about deleting keys, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).

## How many key versions do you have?
{: #pricing-plan-how-many-keys}
{: ui}

To see how many versions you have of each key you have deployed:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Keys** panel, click the ⋯ icon and select **Key details**. This opens a side panel that shows, among other things, the number of versions of this key you have.

5. Repeat this process for every key in your instance. Note that because only root keys can be rotated, all of your standard keys only have one version.
