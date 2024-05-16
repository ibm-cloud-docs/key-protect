---

copyright:
  years: 2024
lastupdated: "2024-05-16"

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

# Pricing for Key Protect on IBM Cloud
{: #pricing-plan}
{: ui}
{: api}
{: cli}

Pricing in {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}} is based on the number of **key versions** in your account. The first five key versions are free, after which there is a charge per month. For the latest monthly charge, check out [{{site.data.keyword.keymanagementserviceshort}} in the {{site.data.keyword.cloud_notm}} catalog](/catalog/services/key-protect).
{: shortdesc}

Creating or importing a key initiates the first **version** of that key. As a result, a newly created or imported key has one version. Each time a key is rotated, a new version is created. A key that has been rotated 10 times, then, has 11 versions (10 versions created by rotations plus the initial version). To discover how many versions you have of each non-deleted key, check out [How many key versions do you have?](/docs/key-protect?topic=key-protect-pricing-plan&interface=ui#pricing-plan-how-many-keys). For more information about rotating keys, check out [Rotating your root keys](/docs/key-protect?topic=key-protect-key-rotation). Note that only root keys can be rotated. For the purpose of {{site.data.keyword.keymanagementserviceshort}} pricing, the [data encryption keys (DEKs)](/docs/key-protect?topic=key-protect-envelope-encryption) used by services or applications to encrypt the data are not counted as key versions in terms of pricing.

While {{site.data.keyword.keymanagementserviceshort}} does not charge a monthly fee for the maintenance of deployed instances, it does charge for the usage of its service through keys that are created, imported, rotated and otherwise managed. Note that "non-deleted" keys refers to any key that has not been moved into the `Destroyed` state. `Deactivated` and `Suspended` keys have not been deleted, and still count as non-deleted versions. For more information about key states, check out [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).

## Pricing scenarios
{: #pricing-plan-scenarios}

For the latest monthly charge, check out [{{site.data.keyword.keymanagementserviceshort}} in the {{site.data.keyword.cloud_notm}} catalog](/catalog/services/key-protect).
{: important}

* **Scenario:** You have one non-deleted key that has been rotated 19 times, which means it has 20 total versions.
  - **Pricing:** You are charged for 15 versions per month (the first five key versions are free).

* **Scenario:** You have one non-deleted key that has been rotated four times, meaning the key has five total versions.
  - **Pricing:** Your charge is USD0 per month.

* **Scenario:** You have one non-deleted key with 20 total versions, another key with three versions, and a key in the `Suspended` state with two versions.
  - **Pricing:** Your are charged for 20 versions per month.

You are charged for all versions of all non-deleted keys that exist in your instances during the billing period (minus the five free key versions). This includes keys you create (or import) and delete before the month is over. If for example you create and delete a key the same day, you are charged the monthly amount for the key (if you have more than five key versions in your instances in that account). However, you are not charged the following month for that key if it remains deleted. If you restore the key at any point, you are charged for all of the versions of that key for that month. For more information about deleting keys, check out [About deleting and purging keys](/docs/key-protect?topic=key-protect-delete-purge-keys).
{: important}

## How many key versions do you have?
{: #pricing-plan-how-many-keys}
{: ui}

To see how many versions you have of each key you have deployed:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Keys** panel, click the ⋯ icon and select **Key details**. This opens a side panel that shows, among other things, the number of versions of this key you have.

5. Repeat this process for every key in your instance. Note that because only root keys can be rotated, all of your standard keys only have one version.
